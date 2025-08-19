## 📉 Diagnóstico de Cancelación de Clientes – Telecom X

### 🧩 Contexto del problema  
Telecom X, empresa del rubro de telecomunicaciones, atraviesa una situación crítica: una alta tasa de cancelación de clientes. Hasta el momento, no se han identificado las causas principales de esta pérdida. En este proyecto, asumes el rol de analista de datos para investigar el fenómeno y aportar soluciones basadas en evidencia.

---

### 📦 Obtención de datos  
Los datos fueron extraídos desde el archivo `TelecomX_Data.json`. Este conjunto incluye información detallada de cada cliente:  
👤 Datos demográficos  
📞 Servicios contratados  
💼 Información de cuenta  
🚪 Estado de cancelación (churn)

---

### 🧪 Preparación del entorno  
Se importaron las librerías necesarias para el análisis y visualización:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Estilo visual para los gráficos
sns.set_style("whitegrid")
```

---

### 📥 Carga de datos  
El archivo JSON se carga en un DataFrame de pandas. Se incluye manejo de errores para asegurar que el archivo esté disponible en el entorno de trabajo:

```python
try:
    df = pd.read_json("/content/drive/MyDrive/Data_Science/Pandas/TelecomX_Data.json")
    print("✅ Datos cargados correctamente.")
    print("📐 Dimensiones del DataFrame:", df.shape)

    print("\n🔍 Primeras 5 filas:")
    display(df.head())

    print("\n📋 Información general del DataFrame:")
    df.info()

except FileNotFoundError:
    print("⚠️ Error: No se encontró el archivo 'TelecomX_Data.json'.")
    print("📎 Por favor, súbelo al entorno de Colab.")
except Exception as e:
    print(f"❌ Se produjo un error: {e}")
```

---

### 📊 Resultado inicial  
Los datos se cargaron con éxito.  
**Tamaño del DataFrame:** 7267 filas × 6 columnas


Primeras 5 filas del DataFrame:

<img width="1145" height="326" alt="image" src="https://github.com/user-attachments/assets/c36d8071-5b01-4de1-b9f4-197ed382b316" />

Información del DataFrame:
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 7267 entries, 0 to 7266
Data columns (total 6 columns):

<img width="374" height="161" alt="image" src="https://github.com/user-attachments/assets/4600afe7-0a78-4f72-82d3-8d5191b190d3" />


## 🔧 Transformación de Datos  
### 3. Limpieza y Preprocesamiento 🧼

---

### 🧱 1. Desanidar la estructura JSON  
Los datos originales contienen columnas con información anidada (diccionarios) que dificultan el análisis directo. Para facilitar el acceso y manipulación, se descompusieron en DataFrames separados:

📊 `customer` → Datos demográficos  
📑 `account` → Información contractual  
📞 `phone` → Servicios telefónicos  
🌐 `internet` → Servicios de internet

```python
df_customer = pd.json_normalize(df['customer'])
df_account = pd.json_normalize(df['account'])
df_phone_services = pd.json_normalize(df['phone'])
df_internet_services = pd.json_normalize(df['internet'])

df_clean = pd.concat([df[['customerID', 'Churn']], df_customer, df_account, df_phone_services, df_internet_services], axis=1)
```

🔗 *Resultado:* Un DataFrame consolidado y plano, listo para análisis exploratorio.

---

### 🚫 2. Limpieza de valores faltantes en `Churn`  
Para asegurar la calidad del análisis, se reemplazaron los valores vacíos en la columna `Churn` por `NaN` y se eliminaron los registros incompletos.

```python
df_clean['Churn'] = df_clean['Churn'].replace('', np.nan)
df_clean.dropna(subset=['Churn'], inplace=True)
```

📌 *Registros válidos restantes:* `7267`

---

### 💸 3. Conversión de `Charges.Total` a formato numérico  
La columna de cargos totales estaba en formato texto y contenía espacios en blanco. Se convirtió a tipo numérico y se rellenaron los valores nulos con `0`, asumiendo que representan clientes nuevos sin cargos acumulados.

```python
df_clean['Charges.Total'] = pd.to_numeric(df_clean['Charges.Total'], errors='coerce')
df_clean['Charges.Total'].fillna(0, inplace=True)
```

📉 *Valores nulos restantes:* `0`

---

### 👵 4. Recodificación de `SeniorCitizen`  
Se transformó la variable binaria `SeniorCitizen` en una categoría legible para facilitar la interpretación:

```python
df_clean['SeniorCitizen'] = df_clean['SeniorCitizen'].map({0: 'No', 1: 'Sí'})
```

🔍 *Valores únicos:* `'No'`, `'Sí'`

---

✅ **Estado final:**  
La limpieza y transformación de los datos se completó con éxito. El DataFrame está listo para análisis visual, segmentación y modelado predictivo.
DataFrame aplanado y combinado:

<img width="539" height="94" alt="image" src="https://github.com/user-attachments/assets/0ab89d0e-1816-4049-98b5-9f068f34c4b2" />
<img width="536" height="101" alt="image" src="https://github.com/user-attachments/assets/b9dccdab-a4c4-4c64-9984-4cb93e6e1a93" />
<img width="531" height="97" alt="image" src="https://github.com/user-attachments/assets/ba91bbc3-cab6-49f1-b853-adc91786667a" />
<img width="443" height="94" alt="image" src="https://github.com/user-attachments/assets/3b257efe-571a-4ddf-b654-b4bf19d684f7" />

## 🧼 Resultados de la Limpieza y Transformación

- 📊 **Tamaño del DataFrame final:** 7043 registros × 21 columnas  
- 🚫 **Registros eliminados por valores vacíos en `Churn`:** 224  
- 💸 **Valores nulos en `Charges.Total` tras la limpieza:** 0  
- 👵 **Categorías únicas en `SeniorCitizen`:** `'No'`, `'Sí'`  
- ✅ **Estado:** Datos listos para análisis exploratorio y visualización

---

### ⚠️ Nota técnica  
Durante la imputación de valores nulos en `Charges.Total`, se generó una advertencia de pandas relacionada con el uso de `.fillna(..., inplace=True)` sobre una copia del DataFrame. Esto se debe a que, en versiones futuras (pandas 3.0), este método no funcionará como se espera.

🔧 **Recomendación:** Para evitar este warning, se puede usar una asignación directa:

```python
df_clean['Charges.Total'] = df_clean['Charges.Total'].fillna(0)
```

Esto asegura que la operación se realice sobre el objeto original sin ambigüedad.



## 🔍 Carga y Análisis Exploratorio (EDA)

### 📌 Paso inicial: Validar la proporción de cancelaciones

Como primer acercamiento al análisis, se visualiza la distribución de clientes que han cancelado el servicio (`Churn`). Esto permite entender el alcance del problema y establecer una línea base para el resto del estudio.

---

### 📊 Visualización: Distribución de Evasión de Clientes

Se utilizó un gráfico de barras para mostrar la cantidad de clientes que permanecen versus los que han cancelado:

```python
plt.figure(figsize=(8, 6))
sns.countplot(x='Churn', data=df_clean, palette='ocean')
plt.title('Distribución de Evasión de Clientes (Churn)', fontsize=16)
plt.xlabel('¿Canceló el servicio?', fontsize=12)
plt.ylabel('Número de Clientes', fontsize=12)

# Añadir porcentajes sobre cada barra
total = len(df_clean)
for p in plt.gca().patches:
    percentage = '{:.1f}%'.format(100 * p.get_height()/total)
    x = p.get_x() + p.get_width() / 2
    y = p.get_height()
    plt.gca().annotate(percentage, (x, y), ha='center', va='bottom', fontsize=12)

plt.show()
```

📌 *Resultado:* Se observa visualmente la proporción entre clientes activos y cancelados, con porcentajes incluidos para facilitar la interpretación.

---

### ⚠️ Nota técnica  
Se generó una advertencia relacionada con el uso del parámetro `palette` sin definir `hue` en `sns.countplot`. Esto será obsoleto en versiones futuras de Seaborn (v0.14.0).

🔧 **Solución recomendada:**  
Si no se necesita leyenda, puedes ajustar el código así:

```python
sns.countplot(x='Churn', data=df_clean, hue='Churn', palette='ocean', legend=False)
```


<img width="696" height="558" alt="image" src="https://github.com/user-attachments/assets/ae395c03-f971-4206-8abf-a0af5f5c7299" />


## 🧬 Análisis Demográfico del Churn

Se evaluó cómo influye el perfil del cliente en la cancelación del servicio:

- 👩‍🦰 **Género**: Comparación entre hombres y mujeres  
- 👵 **Adulto mayor**: Clientes mayores vs. no mayores  
- 👨‍👧 **Dependientes**: Clientes con o sin personas a cargo

Se usaron gráficos de barras para visualizar la proporción de churn en cada grupo. Esto permite identificar segmentos con mayor riesgo y orientar estrategias de retención.

<img width="940" height="264" alt="image" src="https://github.com/user-attachments/assets/0ff542fd-b5a3-4086-a9ac-ada112159237" />


## 📄 Análisis por Contrato y Facturación

Se evaluó cómo influyen tres factores en la cancelación del servicio:

- 📑 **Tipo de contrato**: mensual, anual, etc.  
- 💳 **Método de pago**: tarjeta, transferencia, etc.  
- 📨 **Facturación electrónica**: uso o no de papel

Se generaron gráficos comparativos que muestran la proporción de churn en cada categoría. Esto permite identificar qué condiciones contractuales están más asociadas a la evasión y orientar mejoras en la oferta comercial.

<img width="945" height="285" alt="image" src="https://github.com/user-attachments/assets/474bae36-ef9e-4635-8e52-8f8d604e06d2" />

## 🌐 Análisis por Servicios de Internet

Se evaluó si el tipo de conexión y los servicios adicionales influyen en la cancelación:

- 🚀 **Tipo de internet**: DSL, fibra óptica, etc.  
- 🔐 **Seguridad online**  
- 💾 **Respaldo en línea**  
- 🛠️ **Soporte técnico**

Se filtraron los clientes con servicio de internet y se generaron gráficos comparativos. El objetivo es detectar si la falta de servicios complementarios está asociada a mayor churn.

<img width="915" height="659" alt="image" src="https://github.com/user-attachments/assets/b0ea9da1-6421-45ff-83d3-ef1d12a508fd" />

💸 Análisis por Cargos Mensuales y Totales
Se exploró si los montos facturados influyen en la cancelación del servicio:
- 📆 Cargos mensuales (Charges.Monthly)
- 🧾 Cargos acumulados (Charges.Total)
Se utilizaron gráficos de densidad para comparar la distribución de ambos cargos entre clientes que se quedaron y los que cancelaron. Esto permite detectar si hay rangos de facturación asociados a mayor churn.
<img width="922" height="342" alt="image" src="https://github.com/user-attachments/assets/2b643f39-ddf9-40da-99cb-b3f37fa7a73e" />


## ✅ Informe Final: Conclusiones del Análisis Exploratorio

### 📌 Principales Indicadores de Evasión

- 📅 **Tipo de Contrato**: Los contratos mes a mes presentan la mayor tasa de cancelación.  
- 🌐 **Servicio de Internet**: La fibra óptica está asociada a mayor churn, posiblemente por precio o calidad percibida.  
- 💳 **Método de Pago**: El cheque electrónico muestra fuerte correlación con la evasión.  
- 🔐 **Servicios Adicionales**: La falta de seguridad online, respaldo y soporte técnico aumenta el riesgo.  
- 💸 **Cargos Mensuales**: Montos cercanos a 100 están ligados a mayor cancelación.  
- 👵 **Perfil Demográfico**: Adultos mayores, solteros y sin dependientes son más propensos a cancelar.

---

### 📊 Hallazgos Clave

- Contratos cortos = mayor churn  
- Fibra óptica = servicio premium con alta evasión  
- Cheque electrónico = método de pago crítico  
- Falta de soporte = mayor riesgo  
- Perfil vulnerable = adultos mayores y sin red de apoyo

---

### 🛠️ Recomendaciones

#### Servicios y Contratos  
- Promover contratos de largo plazo  
- Revisar calidad y percepción de la fibra óptica  
- Incentivar servicios de valor agregado (seguridad, respaldo, soporte)

#### Facturación y Pagos  
- Evaluar experiencia de usuarios con facturación electrónica  
- Optimizar procesos de pago con cheque electrónico  
- Analizar impacto de cargos altos en la retención

---

### 📈 Recomendaciones para Ciencia de Datos

- 🗣️ **Análisis de Sentimiento**: Usar encuestas o transcripciones para entender causas profundas  
- 🧪 **Ingeniería de Características**: Combinar variables clave para mejorar el modelo  
- 🤖 **Modelado Predictivo**: Construir modelos de clasificación para anticipar cancelaciones

