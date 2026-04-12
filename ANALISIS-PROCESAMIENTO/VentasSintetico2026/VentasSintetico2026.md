# VENTAS

# 1. DESCRIPCIÓN DEL PROBLEMA

El objetivo del notebook es realizar una auditoría, limpieza y análisis
del dataset.

Se trata de un dataset basado en el caso clásico del Titanic, donde el
problema típico es de clasificación binaria:

-   1 - Sobrevivió
-   0 - No sobrevivió

El dataset contiene datos con problemas intencionales: - Valores nulos -
Tipos de datos inconsistentes - Posibles outliers - Posibles
duplicados - Datos mal formateados

El objetivo principal es detectar, documentar y corregir estos
problemas.

------------------------------------------------------------------------

# 2. RESUMEN GENERAL DEL DATASET

-   Filas: 630
-   Columnas: 8
-   Nulos totales: 72
-   Duplicados: 0
-   Memoria usada (KB): 208.45

## Tipos de datos

-   survived: int64
-   pclass: int64
-   sex: object
-   age: float64
-   fare: object
-   embarked: object
-   booking_date: object
-   ticket: object

------------------------------------------------------------------------

# 3. ANÁLISIS DE ERRORES POR COLUMNA

## Columna: survived

-   Tipo de dato: int64
-   Valores nulos: 0 (0.0%)
-   Valores únicos: 2
-   Posibles outliers (IQR): 0

## Columna: pclass

-   Tipo de dato: int64
-   Valores nulos: 0 (0.0%)
-   Valores únicos: 3
-   Posibles outliers (IQR): 0

## Columna: sex

-   Tipo de dato: object
-   Valores nulos: 0 (0.0%)
-   Valores únicos: 4
-   Columna categórica / texto

## Columna: age

-   Tipo de dato: float64
-   Valores nulos: 44 (6.98%)
-   Valores únicos: 352
-   Posibles outliers (IQR): 2

## Columna: fare

-   Tipo de dato: object
-   Valores nulos: 7 (1.11%)
-   Valores únicos: 538
-   Columna categórica / texto

## Columna: embarked

-   Tipo de dato: object
-   Valores nulos: 21 (3.33%)
-   Valores únicos: 5
-   Columna categórica / texto

## Columna: booking_date

-   Tipo de dato: object
-   Valores nulos: 0 (0.0%)
-   Valores únicos: 630
-   Columna categórica / texto

## Columna: ticket

-   Tipo de dato: object
-   Valores nulos: 0 (0.0%)
-   Valores únicos: 7
-   Columna categórica / texto

------------------------------------------------------------------------

# 4. TRATAMIENTOS REALIZADOS EN EL NOTEBOOK

Se realizaron los siguientes tratamientos:

-   Inspección de valores nulos usando isnull()
-   Conversión de tipos con astype()
-   Uso de fillna() para imputación
-   Revisión de tipos mezclados por columna
-   Auditoría completa por tipo de dato
-   Creación de resumen general del dataset


------------------------------------------------------------------------

# 5. CONTEXTO


El dataset contiene información de pasajeros del Titanic, incluyendo
variables demográficas y del viaje, y el objetivo es predecir si un
pasajero sobrevivió o no al hundimiento. La variable objetivo es
survived, que aunque está como 0 y 1, representa clases (no sobrevivió /
sobrevivió) y se trata como categórica. Las variables de entrada
incluyen: pclass, que indica la clase del pasajero y se considera
categórica ordinal; sex, variable categórica nominal que requiere
limpieza de texto; age, variable numérica continua con valores faltantes
que se imputan con la mediana; fare, que representa el precio del
boleto, se limpia de texto y comas, se convierte a numérica y se imputan
valores faltantes con la media; embarked, variable categórica nominal
que indica el puerto de embarque, con valores faltantes e
inconsistencias, se limpia, se imputan con la moda y se convierte a
categórica; y booking_date, variable de tipo fecha con formatos
inconsistentes que se normaliza a datetime para su correcto uso.

------------------------------------------------------------------------

``` python
import pandas as pd # importamos la libreria pandas.
import numpy as np # importamos la libreria numpy.
from IPython.display import display # importamos la libreria display.

# declaramos una variable para guardar el dataframe.
url = 'https://raw.githubusercontent.com/DevLuis00/Data/refs/heads/main/titanic_sintetico_con_problemas.csv'
df_clean = pd.read_csv(url) # creamos un dataframe donde se realizan los cambios.
df = pd.read_csv(url) # creamos un dataframe para hacer comparaciones.
df_clean.head() # ver el encabezado del dataframe.
```
------------------------------------------------------------------------

## AUDITORIA GENERAL DEL DATAFRAME

``` python
print("\nRESUMEN GENERAL DEL DATASET\n")  
# Imprime un título en consola para indicar que comienza el resumen general del dataset.
# \n agrega un salto de línea antes y después del texto para mejor formato visual.

resumen = pd.DataFrame({
    "Filas": [df.shape[0]],  
    # df.shape[0] obtiene el número de filas del DataFrame.
    # Se coloca dentro de una lista porque el DataFrame espera valores tipo lista.

    "Columnas": [df.shape[1]],  
    # df.shape[1] obtiene el número de columnas del DataFrame.

    "Nulos Totales": [df.isnull().sum().sum()],  
    # df.isnull() convierte el DataFrame en booleanos (True donde hay nulos).
    # .sum() suma los nulos por columna.
    # El segundo .sum() suma todos los nulos del dataset completo.

    "Duplicados": [df.duplicated().sum()],  
    # df.duplicated() devuelve True en filas duplicadas.
    # .sum() cuenta cuántas filas están duplicadas.

    "Memoria (KB)": [round(df.memory_usage(deep=True).sum() / 1024, 2)]  
    # df.memory_usage(deep=True) calcula la memoria usada por cada columna.
    # .sum() suma la memoria total en bytes.
    # /1024 convierte a kilobytes.
    # round(..., 2) redondea a 2 decimales.
})

display(resumen)  
# Muestra el DataFrame resumen en formato tabular (útil en Jupyter Notebook).

print("\nTIPOS DE DATOS\n")  
# Imprime un título indicando que se mostrarán los tipos de datos.

display(df.dtypes)  
# df.dtypes muestra el tipo de dato de cada columna (int64, float64, object, etc.).

print("\nPRIMERAS FILAS DEL DATASET\n")  
# Imprime un título indicando que se mostrarán las primeras filas.

display(df.head())  
# df.head() muestra las primeras 5 filas del DataFrame por defecto.


print("\nAUDITORIA COMPLETA POR COLUMNA\n")  
# Imprime un título indicando que comienza una auditoría más detallada por columna.

resumen_lista = []  
# Se crea una lista vacía donde se almacenará la información detallada de cada columna.

for col in df.columns:  
    # Recorre cada nombre de columna en el DataFrame.

    col_data = df[col]  
    # Extrae los datos de la columna actual.

    tipos_serie = col_data.map(type)  
    # Aplica la función type a cada elemento de la columna.
    # Devuelve una serie con el tipo real de cada valor (ej: int, str, float).

    conteos = tipos_serie.value_counts()  
    # Cuenta cuántas veces aparece cada tipo de dato en la columna.

    conteos.index = conteos.index.map(str)  
    # Convierte los tipos (ej: <class 'int'>) a string para poder manejarlos mejor.

    porcentajes = tipos_serie.value_counts(normalize=True) * 100  
    # value_counts(normalize=True) calcula la proporción en vez del conteo.
    # *100 convierte la proporción a porcentaje.

    porcentajes.index = porcentajes.index.map(str)  
    # Convierte los índices de tipos también a string para que coincidan con conteos.

    for tipo_str in conteos.index:  
        # Recorre cada tipo de dato detectado en la columna.

        resumen_lista.append({
            "Columna": col,  
            # Nombre de la columna analizada.

            "Tipo Detectado": tipo_str,  
            # Tipo de dato encontrado (como string).

            "Cantidad": conteos.loc[tipo_str],  
            # Cantidad de veces que aparece ese tipo en la columna.

            "Porcentaje": f"{porcentajes.loc[tipo_str]:.2f}%"  
            # Porcentaje formateado con 2 decimales y símbolo %.
        })

df_revelador = pd.DataFrame(resumen_lista)  
# Convierte la lista de diccionarios en un nuevo DataFrame estructurado.

display(df_revelador)  
# Muestra el DataFrame final con la auditoría completa por columna.
```
------------------------------------------------------------------------

# **Tratamiento de la columna survived**

------------------------------------------------------------------------
``` python
df_clean['survived'] = df_clean['survived'].map({
    0: 'No sobrevivio',
    1: 'Sobrevivio'
})
# df_clean['survived'] selecciona la columna 'survived' del DataFrame limpio.
# .map({...}) reemplaza los valores numéricos usando un diccionario.
# 0 se transforma en 'No sobrevivio'.
# 1 se transforma en 'Sobrevivio'.
# Es decir, convierte una variable binaria numérica en una variable categórica legible.
df_clean['survived'] = (
    df_clean['survived']
    .astype('category')  
)
# .astype('category') convierte la columna al tipo categórico de pandas.
# Esto es más eficiente en memoria que usar strings normales.
# Además permite que pandas trate la variable como una categoría formal
# (útil para análisis estadístico, modelos y visualizaciones).
```
------------------------------------------------------------------------

# **Tratamiento de la columna Pclass**

------------------------------------------------------------------------
``` python
df_clean['pclass'] = (
    df_clean['pclass']
    .astype(str)        # Convierte todos los valores a tipo string (texto)
    .str.strip()        # Elimina espacios en blanco al inicio y al final
    .astype('category') # Convierte la columna a tipo categórico
)
```
------------------------------------------------------------------------

# **Tratamiento de la columna sex**

------------------------------------------------------------------------
``` python
df_clean['sex'] = (
    df_clean['sex']
    .str.strip()        # Elimina espacios en blanco al inicio y al final
    .str.upper()        # Convierte todo el texto a mayúsculas
    .astype('category') # Convierte la columna a tipo categórico
)
```
------------------------------------------------------------------------

# **Tratamiento de la columna age**

------------------------------------------------------------------------
``` python
df_clean['age'] = (
    df_clean['age']
    .abs()                         # Convierte todos los valores a su valor absoluto
    .fillna(df_clean['age'].mean()) # Reemplaza los valores nulos por la media de la edad
    .astype(int)                   # Convierte los valores a enteros
)
```
------------------------------------------------------------------------

# **Tratamiento de la columna fare**

------------------------------------------------------------------------
``` python
# Convertir a string, quitar espacios y eliminar comas
df_clean['fare'] = (
    df_clean['fare']
    .astype(str)
    .str.strip()
    .str.replace(',', '', regex=False)
)

# Convertir a numérico
df_clean['fare'] = pd.to_numeric(df_clean['fare'], errors='coerce')

# Reemplazar NaN con la media
df_clean['fare'].fillna(df_clean['fare'].mean(), inplace=True)
```
------------------------------------------------------------------------

# **Tratamiento de la columna embarked**

------------------------------------------------------------------------
``` python
df_clean['embarked'] = (
    df_clean['embarked']
    .str.strip()      # Elimina espacios al inicio y al final
    .str.upper()      # Convierte todo a mayúsculas
)

df_clean['embarked'].fillna(
    df_clean['embarked'].mode()[0],   # Reemplaza NaN con la moda (valor más frecuente)
    inplace=True
)

df_clean['embarked'] = df_clean['embarked'].map({
    'S': 'Southampton',
    'C': 'Cherbourg',
    'Q': 'Queenstown'
})  # Reemplaza códigos por nombres completos del puerto

df_clean['embarked'] = (
    df_clean['embarked']
    .str.upper()      # Convierte los nombres a mayúsculas
    .astype('category')  # Convierte la columna a tipo categórico
)
```
------------------------------------------------------------------------

# **Tratamiento de la columna booking_date**

------------------------------------------------------------------------
``` python
df_clean['booking_date'] = pd.to_datetime(
    df_clean['booking_date'],      # Selecciona la columna con las fechas en formato texto
    format='mixed',                # Permite distintos formatos de fecha en la misma columna
    dayfirst=True                  # Indica que el día viene antes que el mes (ej: 25/12/2023)
)  # Convierte la columna a tipo datetime
```
------------------------------------------------------------------------

# **Tratamiento de la columna ticket**

------------------------------------------------------------------------
``` python
df_clean['ticket'] = (
    df_clean['ticket']
    .astype(str)        # Convertir los valores a texto
    .str.strip()        # Eliminar espacios al inicio y al final
    .str.upper()        # Convertir todo a mayúsculas
    .astype('category') # Convertir la columna a tipo categórico
)
```
------------------------------------------------------------------------

# **Resultados**

------------------------------------------------------------------------
``` python
print("=== COMPARACION DE COLUMNAS, FILAS, NAN ===")  
# Título para la comparación general

comparacion = pd.DataFrame({
    "Filas": [df.shape[0], df_clean.shape[0]],  
    # Número de filas del dataset original y limpio

    "Columnas": [df.shape[1], df_clean.shape[1]],  
    # Número de columnas en ambos datasets

    "Nulos": [
        df.isnull().sum().sum(),  
        df_clean.isnull().sum().sum()
    ],  
    # Total de valores NaN en cada dataset

    "Duplicados": [
        df.duplicated().sum(),  
        df_clean.duplicated().sum()
    ],  
    # Total de filas duplicadas

    "Memoria (KB)": [
        round(df.memory_usage(deep=True).sum() / 1024, 2),  
        round(df_clean.memory_usage(deep=True).sum() / 1024, 2)
    ]  
    # Memoria usada por cada dataset en KB
}, index=["Original", "Limpio"])  
# Se agregan etiquetas a las filas para identificar cada dataset

display(comparacion)  
# Muestra la tabla comparativa

print("=== TIPOS DE DATOS ===")  
# Título para la comparación de tipos

tipos = pd.DataFrame({
    "Tipo Original": df.dtypes,  
    # Tipos de datos del dataset original
    "Tipo Limpio": df_clean.dtypes  
    # Tipos de datos después de la limpieza
})

display(tipos)  
# Muestra la comparación de tipos de datos

print("=== PRIMERAS FILAS DATASET ORIGINAL ===")
display(df.head())  
# Muestra las primeras 5 filas del dataset original

print("=== PRIMERAS FILAS DATASET LIMPIO ===")
display(df_clean.head())  
# Muestra las primeras 5 filas del dataset limpio

print("=== DATASET ORIGINAL ===")
print(df.describe())  
# Estadísticas descriptivas del dataset original (numéricas)

print("\n=== DATASET LIMPIO ===")
print(df_clean.describe())  
# Estadísticas descriptivas del dataset limpio
```
------------------------------------------------------------------------