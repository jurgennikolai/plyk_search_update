# Ansible Playbook 

## Descripción
This script does the following: 
1. Searches for files in a specified path.
2. Searches for a textual value within the file.
3. Generates a .csv report detailing the following. 

host | operating system |  file | search | file exists | text found
---- | ---- | ---- | ---- | ---- | ----
... | ... | ... | ... | ... | ...

4. Update the files in two ways: change one textual value to another in the same line or add a new line.
5. Generate a .csv report detailing the changes:

host | operating system | file | search | changeLine | line | changed | failed | backupFile | msg 
---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- 
... | ... | ... | ... | ... | ... | ... | ... | ... | ...

## Preparation
i) The only thing you have to modify are the values of the **pathList** variable, add the paths and changes you want to make. 
To do this, open with your favorite text editor and edit the values of the **pathList** variable in the **playbook.prd.yml.** file.

	- name: Search and Update Files
	  hosts: all
	  become: yes
	  vars:
		pathList:
			- name: '/etc/resolv.conf'
			  search: "nameserver 192.168.1.71"
			  changeLine: false
			  line: "nameserver 192.168.1.2"

			- name: '/etc/temp_file.niko'
			  search: "miEdad\\s*=\\s*NA"
			  changeLine: true
			  line: 'miEdad = 22 '

			- name: '/etc/resolv.confgg'
			  search: "nameserver 192.168.1.1"
			  changeLine: true
			  line: "nameserver 192.168.1.2"
			  
It presents the following structure: 
* **name:** path where the file is located.
* **search:**text to be searched in the file, it accepts regular expressions for a better search.
* **changeLine:** if **false** it means that a new line will be added, if **true** then the line where the searched text is located will be modified.
* **Line:** is the new line that will modify or be added to the file.

#### Interpretation
            - name: '/etc/resolv.conf'
              search: "nameserver 192.168.1.71"
              changeLine: false
              line: "nameserver 192.168.1.2"
This will search the resolv.conf file and look for the following text "nameserver 192.168.1.71". Likewise, having the value **changeLine: false**, it will be added in a new line. 

            - name: '/etc/temp_file.niko'
              search: "miEdad\\s*=\\s*NA"
              changeLine: true
              line: 'miEdad = 22 '
Likewise, the above will look for the temp_file.niko file and search for "miEdad = NA" and change it to "miEdad = 22". That is, if a file has the following form:

		var miEdad = NA

After running the playbook will be:

		var miEdad = 22

## Execution
To run the playbook use the following command:
`$ ansible-playbook playbook.prd.yml -K`

#### Reports
The reports are generated in the same path where the playbook with the names is located: 
 * file_status_report.csv
`$ cat file_status_report.csv`
 * file_update_status_report.csv
`$ cat file_update_status_report.csv`

## Note
The **playbook.dev.yml** and **playbook.prd.yml** files differ in that the first *(dev.)* is for development, i.e., that the logic that generates the report is understandable and can be easily modified. While the second *(prd.)* is for running and getting the right reports.

**This has been tested with Ansible 2.10.** 
**Let's Enjoy! @jurgennikolai**