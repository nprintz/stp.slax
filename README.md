stp.slax
========


#### Description:
Juniper SLAX op script for a quick STP topology breakdown.

This script will combine and shorten the output of both 'show spanning-tree bridge' 
and 'show spanning-tree interface' into one easy to read format. You can determine 
if the local switch is root, what the root priority and mac address are, why the local 
switch is not root, and what interface is the root port if it isn't root. If in a VSTP 
or MSTP scenario, it will also print associated VLANs for each MSTI. 


#### Examples:
**MSTP Example:**

	> op stp 
	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	CIST         32768        3c:8a:b0:9c:3a:01   Yes    N/A                           
	  VLANS: 0-9,51-4094                         
	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	MSTI 1       16385        3c:8a:b0:9c:3a:01   No    Root has lower MAC.    
	                                              Root port:  ge-0/0/13.0         
	  VLANS: 10-20                               
	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	MSTI 2       16385        3c:8a:b0:9c:3a:01   Yes    N/A                           
	  VLANS: 21-50     

**RSTP Example:**
		
	> op stp 
	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	RSTP         8192         40:b4:f0:6a:dc:01   No     Your priority: 32768          
	                                              Root port:  ge-0/0/47.0       
		                                              
#### Version History:
v1.0  -  inital release
v1.1  -  modified template rootport-vlans to not display the 'VLANS' line when RSTP is running, 
			since it should be obvious that all vlans are encompassed by RSTP. 
			Also changed how ouput is handled to remove output-line template, which made code more readable.
v1.2  -  changed invoking of the commands to be one open connection to the RE, to improve callback time.  
		 	Also made some changes to comments. 
