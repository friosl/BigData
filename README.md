# 1. Creación de un Cluster:
# 1.1 Crear una llave:
- Para ello, seleccionamos el servicio EC2 y luego seleccionamos crear una llave, debemos ponerle un nombre y guardar la clave .PEM.
Luego procederemos a crear el cluster:
Accedemos al servicio EMR de AWS, crear un cluster y luego seleccionamos opciones avanzadas.
- Allí en el primer paso seleccionaremos  la versión del EMR que usaremos, la 6.1.0. Luego, los componentes que usaremos, en este caso sólo selecciamos 
 * Hadoop 3.2.1
 * Hive 3.1.2
 * Hue 4.7.1
 * Spark 3.0.0
 * Zeppelin 0.9.0
 * Tez 0.9.2
 * HBase 2.2.5
 * Livy 0.7.0
 * Sqoop 1.4.7.

 (Seleccionamos next)
 Dejamos la Vpc por defeecto, luego seleccionamos en los tipos de instancia los 2 primeros (maestro y principal), que vienen por defecto con m5.xlarge y los cambiamos por m4.large. En "opción de compra" seleccionamos en ambas Spot y damos next.
 
 Colocamos el nombre al cluster y seleccionamos next.
 Seleccionamos el par de claves EC2 que creamos en el paso 1.1 y podemos crear el cluster. Debemos esperar aproximadamente 20 minutos para que se configure.
 
 Además, debemos deshabilitar la opción de "Bloqueo de acceso público" que se encuentra en la parte izquierda del servicio EMR. Y debemos agregar en el rango de puertos el puerto '8888-8888'. Luego, seleccionaremos Ec2 y allí escogeremos el grupo de seguridad donde se encuentra la máquina master, le abriremos los puertos de custom TCP 8888, SSH  (ambos en anywhere) y a medida que vaya siendo necesario usar otras máquinas del cluster podremos abrir sus puertos que encontramos para nuestro uso seleccionando el cluster, Historial de aplicaciones y en 'User interface URL' podremos usar el link que necesitemos y copiarlo en cualquier navegador.
 
De esta manera ya podremos conectarnos a la máquina master ya sea por ssh desde linux y mac usando la clave .PEM que descargamos en el paso 1.1 o en windows usando putty siguiendo este tutorial: https://github.com/st0263eafit/st026320211/blob/main/bigdata/00-aws/GUIA%20PARA%20CONECTARSE%20A%20UNA%20INSTANCIA%20EC2%20USANDO%20SSH.pdf
# 2. Conexión con Hue
 Seleccionamos en el apartado historial de aplicaciones del cluster en EMR, cogemos el link de Hue o "Tonalidad" (en Español) y accedemos a él. La primera vez nos pedirá crear un usuario que será con el que gestionaremos todo el resto. 
 ##  2.1. Gestión de archivos usando la terminal de la máquina master del cluster EC2
Podremos cargar datos de los datasets que se encuentran en el repositorio https://github.com/st0263eafit/st026320211/tree/main/bigdata/datasets
2.2 Crear carpetas vía terminal hacia hue.
Primero debemos acceder vía ssh a la máquina master del cluster. Luego, para crear carpetas, podemos usar el comando hdfs dfs -mkdir /user/<carpeta>
Crearemos un usuario con nueestro nombre (en este caso friosl)-> hdfs dfs -mkdir /user/friosl
 Luego creamos una carpeta dentro de este llamada "datasets"
 -> hdfs dfs -mkdir /user/<usuario>/datasets.
  
 Para verificar que se crearon correctamente, podemos usar el comando 'ls'
  -> hdfs dfs -ls /user y hdfs /dfs -ls /user/friosl.
 Luego podemos utilizar los archivos que descargamos de los datasets y que tenemos que haber subido a la máquina master del cluster, existen varias opciones, en mi caso, usé WinSCP para pasar los archivos y los metí en una carpeta llamada "datasets". Para ello, el archivo debe estar descomprimido.
 
  Para pasar archivos del EMR-master hacia hue usaremos el comando en la terminal del master: (Reemplazar "datasets" por el nombre de la carpeta donde se encuentran).
  hdfs dfs -put /datasets/* /user/<usuario>/datasets
  o
  hdfs dfs -copyFromLocal /datasets/* /user/<usuario>/datasets/
  o si el archivo se encuentra en s3
  hadoop distcp s3a://<link repo s3> /<carpeta destino>
  
#  3. Conexión al DCA vía interfaz gráfica AMBARI:
    Con el usuario/password de la VPN:
    https://hdpambari.dis.eafit.edu.co
#  3.1 Entramos con nuestro usuario y contraseña. Al lado de nuestro perfil encontramos un botón con 9 cuadros y en este seleccionaremos "Files View", este nos llevará a una lista de todas las carpetas de los usuarios de los grupos de tópicos especiales en telemática. Para llegar a nuestra carpeta podemos seleccionar el símbolo de casa que está en el lado superior izquierdo.
  Estando en nuestro directorio crearemos una nueva carpeta con "New Folder" con el nombre de datasets. Entraremos en esa carpeta y allí subiremos los archivos de los datasets. 
#   POR TERMINAL https://hdpssh.dis.eafit.edu.co
  4. Conexión a Hue vía interfaz gráfica
  En la parte izquierda seleccionaremos s3, creamos una carpeta con el nombre de mi usuario 'friosl' y dentro de ella, una carpeta llamada datasets. 
  Para subir archivos a esta carpeta podemos escogerlos de nuestra máquina local seleccionando "upload" y después arrastrándolos o bien escogiéndolos. Cuando terminen de subirse se termina el proceso y quedan subidos consistentemente en s3.

Primero, debemos seleccionar en la sección de AWS el EMR 
