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

**MSTP Example**  

	> op stp 

	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	CIST         8192         00:19:e2:51:83:c1   No     Your priority: 32768          
	                                              Root port: ge-0/0/12.0         
	VLANS: 0-264, 267-269, 271-1300, 1303-1330, 1333-4094

	-------------------------------------------------------------
	Configuration Name     Revision Level    Configuration Digest                    
	Test-MSTP              1                 0x2adca6e344946ceedde262329c949bb0      

	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	MSTI 1       32769        00:19:e2:51:85:c1   No     Root has lower MAC.           
	VLANS: 1301-1302                              Root port: ge-0/0/12.0         

	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	MSTI 2       32770        3c:8a:b0:9b:fc:81   Yes    N/A                           
	No active interfaces on any VLANs in this MSTI.
	VLANS: 1331-1332                             

	-------------------------------------------------------------
	Region       Rt Priority  Root MAC            Root?  Why Not?                      
	MSTI 3       32771        00:19:e2:51:85:c1   No     Root has lower MAC.           
	VLANS: 265-266, 270                           Root port: ge-0/0/12.0  

**VSTP Example**  

	> op stp 

	VSTP Instances:
	VSTP VLAN    Rt Priority  Root MAC            Root?  Why Not?                      
	 Vlan 10     32778        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 11     32779        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 12     32780        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 13     32781        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 1301   34069        00:19:e2:51:85:c1   No     Root has lower MAC.           
	                                              Root port: ge-0/0/12.0         
	 Vlan 1331   34099        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 14     32782        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 15     32783        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 16     32784        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 17     32785        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 18     32786        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 21     32789        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 23     32791        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 24     32792        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 258    33026        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 265    33033        00:19:e2:51:85:c1   No     Root has lower MAC.           
	                                              Root port: ge-0/0/12.0         
	 Vlan 400    33168        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 500    33268        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 620    33388        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 621    33389        3c:8a:b0:9b:fc:81   Yes    N/A                           
	 Vlan 666    33434        3c:8a:b0:9b:fc:81   Yes    N/A  

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
	- Updated example output inside the script, and within the Readme file. 
	