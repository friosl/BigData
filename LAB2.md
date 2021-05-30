Para esto debemos haber hecho el laboratorio 1.

Word-Count con Spark y python.

## 1. Cómo usar pyspark a partir del EMR master o a partir del DCA ( https://hdpssh.dis.eafit.edu.co )

Usaremos el siguiente código:

$ pyspark
files_rdd = sc.textFile("hdfs:///datasets/gutenberg-small/*.txt") #Archivo donde están guardados los datos 

wc_unsort = files_rdd.flatMap(lambda line: line.split()).map(lambda word: (word, 1)).reduceByKey(lambda a, b: a + b)

wc = wc_unsort.sortBy(lambda a: -a[1])

for tupla in wc.take(10):

   print(tupla)
    
wc.saveAsTextFile("hdfs:///tmp/frioslwcout1") #Escribir archivo


#Escribí todo en un sólo archivo:

>>> wc.coalesce(1).saveAsTextFile("hdfs:///tmp/frioslwcout2")


## 2. Desde Zeppelin Notebook (EAFIT)
Nos loggeamos, le damos en el botón de Notebook y  luego en "New Note"
Ejecutamos:

%spark2.pyspark

WORDCOUNT COMPACTO

files_rdd = sc.textFile("s3://emontoyapublic/datasets/gutenberg-small/*.txt")

files_rdd = sc.textFile("hdfs:///datasets/gutenberg-small/*.txt")

wc_unsort = files_rdd.flatMap(lambda line: line.split()).map(lambda word: (word, 1)).reduceByKey(lambda a, b: a + b)

wc = wc_unsort.sortBy(lambda a: -a[1])

for tupla in wc.take(10):

   print(tupla)
    
wc.coalesce(1).saveAsTextFile("hdfs:///tmp/frioslwcout3")

Ya está creado un archivo 3.

## 3. En JupyterNotebooks usando el servicio EMR de AWS:

Teniendo desplegado un cluster, lo seleccionamos. A la izquierda nos encontramos la opción de "Bloc de notas" o "Notebook"
Creamos el notebook en el mismo cluster y en la dirección de s3 donde tenemos nuestro bucket, aunque también podemos dejar el por que está por defecto.

Usaremos este código:

files_rdd = sc.textFile("s3://feliper/st0263feliper/datasets/gutenberg-small/*.txt")

#files_rdd = sc.textFile("hdfs:///user/emontoya/datasets/gutenberg-small/*.txt")

wc_unsort = files_rdd.flatMap(lambda line: line.split()).map(lambda word: (word, 1)).reduceByKey(lambda a, b: a + b)

wc = wc_unsort.sortBy(lambda a: -a[1])

for tupla in wc.take(10):

   print(tupla)
        
#Salvar los datos de salida, si no existen hdfs:///tmp/frioslwcout4

wc.coalesce(1).saveAsTextFile("hdfs:///tmp/frioslwcout4")

#si esta trabajando en aws (igual verifique que no exista previamente wcout4):

wc.coalesce(1).saveAsTextFile("s3://feliper/st0263feliper/wcout4")

