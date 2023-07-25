# Ansible Playbook 

## Descripción
Este script realiza lo siguiente: 
1. Busca los archivos en una ruta especifica.
2. Busca un valor textual dentro del archivo.
3. Genera un reporte en .csv detallando lo siguiente. 

host | operating system | file | search | changeLine | line | changed | failed | backupFile | msg 
---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- 
... | ... | ... | ... | ... | ... | ... | ... | ... | ...

4. Realiza la actualización en los archivos de dos maneras: cambiar un valor textual por otra en una misma linea o agregar una nueva linea.
5. Genera un reporte en .csv detallando los cambios:

host | operating system | file | search | changeLine | line | changed | failed | backupFile | msg 
---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- 
... | ... | ... | ... | ... | ... | ... | ... | ... | ...

## Preparación
i) Lo único que se debe modificar son los valores de la variable **pathList** , agregar las rutas y cambios que se quiere realizar. 
Para ello, abrir con tu editor de texto favorito y editar los valores de la variable **pathList** del archivo **playbook.prd.yml.**

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
			  
El cual presenta presenta la siguiente estructura: 
* **name:** ruta donde se ubica el archivo
* **search:**texto a ser buscado en el archivo, acepta expresiones regulares para una mejor busqueda.
* **changeLine:** si es **false** significa que se agregará una nueva linea, si es **true** entonces se modificará la linea donde se encuentra el texto buscado.
* **Line:** es la nueva linea que modificará o será agregado en el archivo.

#### Interpretación
            - name: '/etc/resolv.conf'
              search: "nameserver 192.168.1.71"
              changeLine: false
              line: "nameserver 192.168.1.2"
Lo anterior se encargará de buscar el archivo resolv.conf y buscará el siguiente texto "nameserver 192.168.1.71". Así mismo, al tener el valor **changeLine: false**, se agregará en una nueva linea. 

            - name: '/etc/temp_file.niko'
              search: "miEdad\\s*=\\s*NA"
              changeLine: true
              line: 'miEdad = 22 '
Así mismo, lo anterior se encargará de buscar el archivo temp_file.niko y buscará "miEdad  =  NA" para cambiarlo por "miEdad = 22". Es decir, si un archivo tiene la siguiente forma:

		var miEdad = NA

Despues de ejecutar el playbook será: 

		var miEdad = 22

## Ejecución
Para ejecutar el playbook  usar el siguiente comando:
`$ ansible-playbook playbook.prd.yml -K`

#### Reportes
Los reportes se generan en la misma ruta donde se encuentra el playbook con los nombres: 
 * file_status_report.csv
`$ cat file_status_report.csv`
 * file_update_status_report.csv
`$ cat file_update_status_report.csv`

## Nota
El archivo **playbook.dev.yml** y **playbook.prd.yml** se diferencian en que la primera *(dev.)* es para desarrollo, es decir, que la lógica que genera el reporte sea entendible y pueda ser modificado con facilidad. Mientras que la segunda *(prd.)* es para ejecutar y obtener los reportes adecuados.

**Esto ha sido probado con Ansible 2.10**
**¡A usarlo! @jurgennikolai**
