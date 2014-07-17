stp.slax
========

#### Description:
This is a Juniper SLAX op script to provide a quick STP topology breakdown.

This script will combine and shorten the output of the `show spanning-tree bridge`, 
`show spanning-tree interface`, and `show spanning-tree mstp configuration` 
commands into one easy to read format. You can determine 
if the local switch is root, what the root priority and root MAC address are, why the local 
switch is not root, and what interface is the root port if it isn't root. If in a VSTP 
or MSTP scenario, it will also print associated VLANs for each MSTI. For MSTP configurations, it will
also output the Configuration Name, Revision Level, and Configuration Digest. 

For detailed information for storing and enabling an op script on your Junos device, please visit [this page](http://www.juniper.net/techpubs/en_US/junos13.2/topics/task/configuration/junos-script-automation-script-storing-enabling.html)

[General SLAX Overview](http://www.juniper.net/techpubs/en_US/junos14.1/topics/concept/junos-script-automation-slax-overview.html)  
[Junos Automation Documentation](http://www.juniper.net/techpubs/en_US/junos13.2/information-products/pathway-pages/config-guide-automation/configuration-and-operations-automation.html#overview)  

#### Examples:
**RSTP Example:**  
	
	> op stp 

	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	RSTP         8192         40:b4:f0:6a:dc:01   No     Your priority: 32768          
	                                              Root port:  ge-0/0/47.0       
		                                              
**MSTP Example:**  
	
	> op stp 

	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	CIST         32768        3c:8a:b0:9c:3a:01   Yes    N/A                           
	  VLANS: 0-9,51-4094                         

	-------------------------------------------------------------
	Configuration Name     Revision Level    Configuration Digest                    
	Test-MSTP              1                 0x2adca6e344946ceedde262329c949bb0      

	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	MSTI 1       16385        3c:8a:b0:9c:3a:01   No    Root has lower MAC.    
	  VLANS: 10-20                                Root port:  ge-0/0/13.0         

	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	MSTI 2       16385        3c:8a:b0:9c:3a:01   Yes    N/A                           
	  VLANS: 21-50     

#### Version History:
* v1.0
	- Inital Release
* v1.1
	- Modified template rootport-vlans to not display the 'VLANS' line when RSTP is running, since it should be obvious that all vlans are encompassed by RSTP. 
	- Also changed how ouput is handled to remove output-line template, which made code more readable.
* v1.2
	- Changed invoking of the commands to be one open connection to the RE, to improve callback time. 
	- Also made some changes to comments. 
* v1.3 
	- Added blank lines between MSTI regions for readability. 
	- Modified the output of the VLAN list. If it is greater than 35 characters, 
	it shows up one line below, so that the Root Port information doesn't get shifted right.
	- Added output for the MSTP Configuration Name, Revision Level, and Configuration Digest. 
	- Fixed the logic for MSTP instances. There was an error so that the first MSTI was being used as the results for all MSTI instances. 
	- Updated the Readme with instructions on using the SLAX script. 
	- Modified output to be shown to the user as it is generated, rather than all at once upon script completion.
	This should create quicker results for large numbers of MSTIs, so the user knows that the script is actively working.
* v1.3.1
	- Rootport-vlans template will now let the user know if there are no interfaces active in a given MSTI. 
	- Rootport-vlans template will also now output the VLAN info for MSTIs where no VLANS in the MSTI have active interfaces. 
	- VLAN info will now also be omitted when the protocol is STP, was previously only omitting for RSTP. 
	- VSTP logic fix, had the same problem as MSTP in v1.3
	- Made the VLAN list output easier to read by injecting a space after each comma. 