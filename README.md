
# 📊 Análisis de Evasión de Clientes - Telecom X

## 🧩 Descripción del Problema

Telecom X, una empresa del sector de telecomunicaciones, está enfrentando un alto índice de evasión de clientes (churn). A pesar de contar con una base de datos amplia, aún no han logrado identificar con claridad los factores que impulsan esta pérdida. El objetivo de este proyecto es actuar como analista de datos para descubrir patrones, correlaciones y posibles causas de la evasión, con el fin de proponer estrategias que mejoren la retención de clientes.

## 📁 Extracción de Datos

Los datos fueron extraídos del archivo `TelecomX_Data.json`, el cual contiene:

- Información demográfica de los clientes (edad, género, ubicación)
- Servicios contratados (internet, telefonía, televisión, etc.)
- Detalles de la cuenta (antigüedad, tipo de contrato, método de pago)
- Indicador de cancelación de suscripción (`churn`: sí/no)

Este conjunto de datos permite realizar un análisis exploratorio profundo y construir modelos predictivos para anticipar la evasión.

## 🧪 Preparación del Entorno

Se importaron las siguientes librerías para el análisis y visualización de datos:

```python

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Configuración de estilo para los gráficos
sns.set_style("whitegrid")
```

## 📥 Carga de Datos

Se realizó la carga del archivo `TelecomX_Data.json` en un DataFrame de pandas utilizando Google Colab. Este archivo contiene información clave sobre los clientes de Telecom X, incluyendo:

- Datos demográficos
- Servicios contratados
- Información de la cuenta
- Indicador de cancelación (`churn`)

### 🧾 Código utilizado para la carga:

```python
try:
    # Cargar los datos desde el archivo JSON
    df = pd.read_json("/content/drive/MyDrive/Data_Science/Pandas/TelecomX_Data.json")
    print("Datos cargados exitosamente.")
    print("Dimensiones del DataFrame:", df.shape)

    # Mostrar las primeras filas para entender la estructura
    display(df.head())

    # Ver la información general del DataFrame
    df.info()

except FileNotFoundError:
    print("Error: El archivo 'TelecomX_Data.json' no se encontró.")
    print("Please upload the 'TelecomX_Data.json' file to your Colab environment.")
except Exception as e:
    print(f"An error occurred: {e}")
```

### 📊 Resultado de la carga:

- ✅ Datos cargados exitosamente  
- 📐 Dimensiones del DataFrame: 7,267 filas × 6 columnas  
- 👀 Visualización inicial: primeras 5 filas y estructura general del DataFrame

Primeras 5 filas del DataFrame:
customerID	Churn	customer	phone	internet	account
0	0002-ORFBO	No	{'gender': 'Female', 'SeniorCitizen': 0, 'Part...	{'PhoneService': 'Yes', 'MultipleLines': 'No'}	{'InternetService': 'DSL', 'OnlineSecurity': '...	{'Contract': 'One year', 'PaperlessBilling': '...
1	0003-MKNFE	No	{'gender': 'Male', 'SeniorCitizen': 0, 'Partne...	{'PhoneService': 'Yes', 'MultipleLines': 'Yes'}	{'InternetService': 'DSL', 'OnlineSecurity': '...	{'Contract': 'Month-to-month', 'PaperlessBilli...
2	0004-TLHLJ	Yes	{'gender': 'Male', 'SeniorCitizen': 0, 'Partne...	{'PhoneService': 'Yes', 'MultipleLines': 'No'}	{'InternetService': 'Fiber optic', 'OnlineSecu...	{'Contract': 'Month-to-month', 'PaperlessBilli...
3	0011-IGKFF	Yes	{'gender': 'Male', 'SeniorCitizen': 1, 'Partne...	{'PhoneService': 'Yes', 'MultipleLines': 'No'}	{'InternetService': 'Fiber optic', 'OnlineSecu...	{'Contract': 'Month-to-month', 'PaperlessBilli...
4	0013-EXCHZ	Yes	{'gender': 'Female', 'SeniorCitizen': 1, 'Part...	{'PhoneService': 'Yes', 'MultipleLines': 'No'}	{'InternetService': 'Fiber optic', 'OnlineSecu...	{'Contract': 'Month-to-month', 'PaperlessBilli...
Información del DataFrame:
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 7267 entries, 0 to 7266
Data columns (total 6 columns):
 #   Column      Non-Null Count  Dtype 
---  ------      --------------  ----- 
 0   customerID  7267 non-null   object
 1   Churn       7267 non-null   object
 2   customer    7267 non-null   object
 3   phone       7267 non-null   object
 4   internet    7267 non-null   object
 5   account     7267 non-null   object
dtypes: object(6)
memory usage: 340.8+ KB

