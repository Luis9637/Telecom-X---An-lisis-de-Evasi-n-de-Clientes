
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

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Configuración de estilo para los gráficos
sns.set_style("whitegrid")
