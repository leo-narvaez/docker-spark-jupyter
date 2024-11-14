Paso 1: Iniciar el contenedor y acceder a Jupyter
Inicia el contenedor si no lo has hecho ya, utilizando Docker Compose:

bash
Copiar código
docker-compose up --build
Una vez que el contenedor esté en funcionamiento, abre tu navegador y accede a Jupyter Notebook en http://localhost:8888.

Paso 2: Crear un notebook de Scala
Seleccionar el kernel de Scala:

Al abrir Jupyter, deberías poder ver un botón que dice New en la parte superior derecha. Al hacer clic allí, verás una opción llamada Scala, que corresponde al kernel de Scala instalado a través de Apache Toree.

Crear un nuevo notebook con el kernel de Scala:

Selecciona Scala como el kernel para el notebook y se abrirá una nueva pestaña donde podrás comenzar a escribir código en Scala.

Ejemplo avanzado con Scala y Spark
Vamos a crear un DataFrame más complejo, simular algunas operaciones de transformación y luego ejecutar consultas SQL sobre este DataFrame.

Ejemplo en Scala:
scala
Copiar código
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.functions._

// Crear una sesión de Spark
val spark = SparkSession.builder
  .appName("Ejemplo avanzado en Scala con Spark")
  .master("spark://spark-jupyter:7077") // Asegúrate de que el master es el correcto
  .getOrCreate()

// Datos más complejos
val data = Seq(
  ("John", "Doe", 30, "M", "New York"),
  ("Alice", "Smith", 25, "F", "Los Angeles"),
  ("Bob", "Brown", 28, "M", "Chicago"),
  ("Eve", "Davis", 22, "F", "Miami"),
  ("Charlie", "Wilson", 35, "M", "San Francisco")
)

// Crear un DataFrame con columnas más complejas
val df = spark.createDataFrame(data).toDF("first_name", "last_name", "age", "gender", "city")

// Mostrar el DataFrame original
df.show()

// Registrar el DataFrame como una vista temporal para SQL
df.createOrReplaceTempView("people")

// Realizar una consulta SQL avanzada para filtrar, agrupar y ordenar
val result = spark.sql("""
  SELECT city, gender, COUNT(*) as count, AVG(age) as avg_age
  FROM people
  WHERE age > 25
  GROUP BY city, gender
  ORDER BY avg_age DESC
""")

// Mostrar los resultados
result.show()
Explicación:
DataFrame Complejo: Creamos un DataFrame con más columnas (nombre, apellido, edad, género, ciudad).
Transformaciones y Consultas: Usamos spark.sql() para realizar una consulta SQL que filtra personas mayores de 25 años, agrupa los resultados por ciudad y género, y luego ordena los resultados por la edad promedio en orden descendente.
Salida: El DataFrame resultante contiene las ciudades y géneros con el número de personas y la edad promedio, ordenado por la edad promedio.
Ejemplo equivalente con PySpark:
El equivalente en PySpark sería muy similar, pero con la sintaxis de Python. A continuación te muestro cómo hacer lo mismo.

Ejemplo en PySpark:
python
Copiar código
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, avg, count

# Crear la sesión de Spark
spark = SparkSession.builder \
    .appName("Ejemplo avanzado en PySpark") \
    .master("spark://spark-jupyter:7077") \
    .getOrCreate()

# Datos más complejos
data = [
    ("John", "Doe", 30, "M", "New York"),
    ("Alice", "Smith", 25, "F", "Los Angeles"),
    ("Bob", "Brown", 28, "M", "Chicago"),
    ("Eve", "Davis", 22, "F", "Miami"),
    ("Charlie", "Wilson", 35, "M", "San Francisco")
]

# Crear un DataFrame con columnas más complejas
df = spark.createDataFrame(data, ["first_name", "last_name", "age", "gender", "city"])

# Mostrar el DataFrame original
df.show()

# Registrar el DataFrame como una vista temporal para SQL
df.createOrReplaceTempView("people")

# Realizar una consulta SQL avanzada para filtrar, agrupar y ordenar
result = spark.sql("""
  SELECT city, gender, COUNT(*) as count, AVG(age) as avg_age
  FROM people
  WHERE age > 25
  GROUP BY city, gender
  ORDER BY avg_age DESC
""")

# Mostrar los resultados
result.show()
Explicación:
Crear DataFrame en PySpark: Usamos spark.createDataFrame() para crear el DataFrame a partir de una lista de tuplas.
Registrar la vista temporal: Al igual que en el ejemplo de Scala, registramos el DataFrame como una vista temporal llamada people.
Consulta SQL avanzada: Realizamos una consulta SQL usando spark.sql() para filtrar, agrupar y ordenar los resultados.
Mostrar los resultados: Mostramos el resultado con el método show().
