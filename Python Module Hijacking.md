Funcionamiento del Ataque



    Orden de Búsqueda de Módulos:


        Python busca primero en el directorio actual desde donde se ejecuta el script. Si encuentra un archivo con el mismo nombre que el módulo que se intenta importar, lo cargará en lugar del módulo original.




    Creación de un Módulo Malicioso:


        Al crear un archivo con el mismo nombre que un módulo legítimo (por ejemplo, requests.py), se puede inyectar código malicioso que se ejecutará cuando el script intente importar ese módulo.




    Ejecución del Código Malicioso:


        Si el script se ejecuta con privilegios elevados (por ejemplo, usando sudo), el código malicioso se ejecutará con esos privilegios, lo que puede comprometer la seguridad del sistema.



