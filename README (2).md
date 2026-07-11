# 📡 ConnectaTel — Análisis de Comportamiento de Uso de Clientes

## 📋 Descripción del proyecto

ConnectaTel es una empresa de telecomunicaciones con operaciones en México y Colombia. Este proyecto analiza cómo los clientes utilizan realmente los servicios móviles (llamadas y mensajes) durante el año 2024, con el objetivo de identificar patrones de uso, detectar comportamientos atípicos y comprender qué segmentos de clientes muestran necesidades diferenciadas, para así optimizar la oferta comercial y mejorar la experiencia del usuario.

## 💡 Preguntas de negocio

- ¿Qué segmentos de clientes muestran mayor o menor uso de llamadas y mensajes?
- ¿Qué usuarios presentan valores atípicos que puedan indicar comportamientos inusuales, fraude o errores de registro?
- ¿Cómo varía el uso según la edad y el tipo de plan contratado?
- ¿Qué patrones pueden ayudar a diseñar mejores planes, optimizar la oferta y mejorar la satisfacción del cliente?

## 🗂️ Datasets utilizados

| Archivo | Descripción |
|---|---|
| `plans.csv` | Catálogo de los 2 planes actuales (Básico, Premium): precio, minutos incluidos, GB incluidos, costo por extra. |
| `users_latam.csv` | Información de los 4,000 clientes: edad, ciudad, fecha de registro, plan contratado, fecha de churn. |
| `usage.csv` | Detalle de 40,000 registros de uso real: llamadas (duración) y mensajes (longitud). |

## 🛠️ Herramientas

- Python 3.9
- Jupyter Notebook
- pandas, numpy
- seaborn, matplotlib

## 🔄 Estructura del análisis

1. **Carga y exploración** — lectura de los 3 datasets y revisión inicial de estructura y tipos de datos.
2. **Identificación de problemas de calidad** — nulos, sentinels y fechas fuera de rango.
3. **Limpieza básica** — corrección de sentinels (`-999` en `age`, `"?"` en `city`), estandarización de fechas y tratamiento de nulos MAR en `usage`.
4. **Summary statistics** — construcción de `user_profile` (perfil agregado de uso por cliente) y estadísticas descriptivas.
5. **Visualización y outliers** — histogramas por plan, boxplots y detección de outliers con el método IQR.
6. **Segmentación** — clasificación de clientes por `grupo_uso` (Bajo/Medio/Alto) y `grupo_edad` (Joven/Adulto/Adulto Mayor).
7. **Insight ejecutivo** — conclusiones y recomendaciones comerciales para stakeholders.

## 🔎 Problemas de calidad de datos encontrados

- **`age`**: valor sentinel `-999` → reemplazado por la mediana.
- **`city`**: 565 registros (14.1%) sin ciudad válida (nulos + sentinel `"?"`) → unificados como valor faltante.
- **`reg_date`**: 40 registros (1%) con año 2026 (fecha futura imposible) → marcados como nulos.
- **`duration`/`length`** en `usage`: nulos estructurales (MAR), dependientes de la columna `type` — no se imputan, se mantienen como NA.

## 📊 Principales hallazgos

- El **73.6%** de los clientes se ubica en el segmento de **"Uso medio"**, 19.5% en "Bajo uso" y 7.0% en "Alto uso".
- La **edad no correlaciona** con el nivel de uso: la proporción de "Alto uso" es similar (~7%) en los tres grupos etarios.
- El plan contratado (Básico 64.9% vs. Premium 35.1%) **no explica diferencias reales de consumo** — los promedios de mensajes y llamadas son casi idénticos entre ambos planes.
- Se detectaron outliers de alto consumo, especialmente en minutos de llamada: **109 usuarios (2.7%)** superan el límite IQR de 61.9 minutos, llegando hasta 155.7 minutos — posible oportunidad comercial para un plan de mayor consumo.

## 💡 Recomendaciones clave

1. Crear un plan de **"Alto Consumo"** de minutos dirigido a los usuarios que ya exceden el uso típico.
2. Diseñar una **campaña de upgrade a Premium** dirigida al segmento de "Alto uso" (7% de la base).
3. **No segmentar comercialmente por edad**, dado que no muestra relación con el nivel de consumo.
4. Mejorar la **captura de datos en origen** (ciudad y fecha de registro) para análisis futuros más confiables.

## ▶️ Cómo ejecutar el notebook

### Opción A: Google Colab (recomendada, sin instalación)

1. Sube el archivo `connectatel_analysis.ipynb` a [Google Colab](https://colab.research.google.com/) (`Archivo` → `Subir cuaderno`), o ábrelo directamente desde GitHub una vez subido el repositorio (`Archivo` → `Abrir cuaderno` → pestaña `GitHub`).
2. Sube los 3 datasets (`plans.csv`, `users_latam.csv`, `usage.csv`) a la sesión de Colab: en el panel izquierdo, ícono de carpeta → `Subir` → crea una carpeta `datasets` y coloca los archivos ahí, o ajusta las rutas de `pd.read_csv()` según donde los subas.
3. Ejecuta las celdas en orden con `Entorno de ejecución` → `Ejecutar todas`. Las librerías (`pandas`, `numpy`, `seaborn`, `matplotlib`) ya vienen preinstaladas en Colab.

### Opción B: Jupyter Notebook local

```bash
# Clonar el repositorio
git clone <url-del-repositorio>
cd connectatel-analysis

# Instalar dependencias
pip install pandas numpy seaborn matplotlib

# Abrir el notebook
jupyter notebook connectatel_analysis.ipynb
```

**Nota:** los archivos `plans.csv`, `users_latam.csv` y `usage.csv` deben ubicarse en una carpeta `/datasets` en la raíz del proyecto para que las rutas de carga (`pd.read_csv('/datasets/...')`) funcionen correctamente.

## 🔁 Guía rápida de reproducción

1. Clona o descarga este repositorio.
2. Asegúrate de tener los 3 datasets (`plans.csv`, `users_latam.csv`, `usage.csv`) en la carpeta `datasets/`.
3. Abre `connectatel_analysis.ipynb` en Colab o Jupyter (ver opciones arriba).
4. Ejecuta el notebook de principio a fin, en orden — cada sección depende de las variables creadas en la anterior (`plans` → `users` → `usage` → `user_profile`).
5. Revisa la sección final **"Insight Ejecutivo para Stakeholders"** para las conclusiones y recomendaciones comerciales.

## 📁 Estructura del repositorio

```
connectatel-analysis/
│
├── datasets/
│   ├── plans.csv
│   ├── users_latam.csv
│   └── usage.csv
│
├── connectatel_analysis.ipynb
├── README.md
└── requirements.txt
```

## ✍️ Autora

Romina — Fonoaudióloga en transición a Analista de Datos, especializada en el sector salud.
