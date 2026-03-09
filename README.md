# TelecomX LATAM — Análisis de Evasión de Clientes (Churn)

Este es un análisis exploratorio de datos, para poder identificar los factores que llevan a la cancelación de clientes en TelecomX LATAM, con el objetivo de diseñar estrategias de retención basadas en evidencia.

---

## Descripción del Proyecto

Empezamos con la evasión de clientes (*churn*) que es uno de los problemas más costosos para las empresas de telecomunicaciones. El costo de poder adquirir un cliente nuevo es entre **5 y 7 veces mayor** que el de retener uno existente.

Por el momento TelecomX LATAM enfrenta este desafío directamente: aproximadamente **1 de cada 4 clientes cancela su servicio** (tasa de churn del 26.5%). Este proyecto analiza los datos de 7,267 clientes para entender **quién cancela, por qué y cuándo**, y traduce esos hallazgos en recomendaciones concretas.

---

## Objetivos

- Identificar los factores demográficos, contractuales y de comportamiento más asociados a la cancelación
- Visualizar patrones de evasión a través de variables categóricas y numéricas
- Definir el perfil de cliente con mayor riesgo de churn
- Proponer estrategias de retención basadas en los datos

---

## Estructura del Proyecto

```
TelecomX_LATAM/
│
├── TelecomX_LATAM.ipynb     # Notebook principal con el análisis completo
└── README.md                # Este archivo
```

---

## Dataset

| Característica | Detalle |
|---|---|
| Fuente | API pública GitHub (formato JSON) |
| Registros originales | 7,267 clientes |
| Registros tras limpieza | 7,043 clientes |
| Variables | 22 columnas |
| Variable objetivo | `Evasion` (1 = canceló, 0 = permaneció) |

**Tipos de variables:**
- **Demográficas:** Género, Adulto Mayor, Pareja, Dependientes
- **Servicios:** Teléfono, Internet, Seguridad Online, Streaming, Soporte Técnico
- **Contrato:** Tipo de contrato, Método de pago, Factura digital
- **Facturación:** Cargos mensuales, Cargos totales, Cuentas diarias (variable derivada)

---

## Tecnologías utilizadas

- **Python 3**
- **Pandas** — manipulación y limpieza de datos
- **Requests** — extracción de datos desde API
- **Matplotlib** — visualizaciones
- **Seaborn** — heatmap de correlación
- **Google Colab** — entorno de ejecución

---

## Pipeline ETL

### 1. Extracción
- Datos cargados desde API pública en formato JSON
- Validación HTTP con `response.raise_for_status()`
- Conversión a DataFrame con `pd.json_normalize()`

### 2. Limpieza
| Problema detectado | Columna | Solución |
|---|---|---|
| Valores vacíos `''` | `Churn` | `replace('', pd.NA)` + `dropna()` → 224 registros eliminados |
| Tipo incorrecto (`string`) | `account.Charges.Total` | `pd.to_numeric(..., errors='coerce')` |
| Espacios y mayúsculas | Todas las columnas de texto | `.str.strip().str.lower()` |

### 3. Transformación
- 21 columnas renombradas al español
- Variables binarias `Yes/No` convertidas a `1/0`
- Valores categóricos traducidos al español
- Variable derivada `Cuentas_Diarias = Cargos_Mensuales / 30`

---

## Principales Hallazgos

### Tasa de Evasión General
| Estado | Clientes | Porcentaje |
|---|---|---|
| Permanecieron | ~5,174 | 73.5% |
| Cancelaron | ~1,869 | **26.5%** |

### Variables Categóricas más relevantes
| Variable | Mayor evasión | Tasa | Menor evasión | Tasa |
|---|---|---|---|---|
| Tipo de contrato | Mes a mes | ~43% | Dos años | ~3% |
| Método de pago | Cheque electrónico | ~45% | Transferencia bancaria | ~17% |
| Servicio de internet | Fibra óptica | ~42% | Sin internet | ~7% |
| Factura digital | Sí | ~33% | No | ~16% |

### Variables Numéricas comparadas
| Variable | Permanecen (media) | Cancelan (media) |
|---|---|---|
| Meses de contrato | ~37 meses | ~18 meses |
| Cargos mensuales | ~$61 | ~$74 |
| Cargos totales | ~$2,555 | ~$1,532 |

---

## Insights Clave

1. **Los primeros 12 meses son la ventana crítica** — los clientes que cancelan tienen en promedio de solo 18 meses de antigüedad
2. **El tipo de contrato es el predictor más poderoso** — los contratos mes a mes tienen una tasa de churn 14 veces mayor que los de los dos años
3. **Fibra óptica tiene un problema de valor percibido** — es el servicio más caro y el de mayor evasión (~42%)
4. **El método de pago refleja el nivel de compromiso** — cheque electrónico lidera la evasión (~45%)
5. **Los clientes sin vínculos familiares son más vulnerables** — sin pareja (~33%) y sin dependientes (~31%)
6. **El perfil de máximo riesgo es identificable** — contrato mes a mes + fibra óptica + cheque electrónico + menos de 12 meses de antigüedad

---

## Recomendaciones Estratégicas

| Prioridad | Acción |
|---|---|
| 1 | Programa de fidelización activo durante los primeros 12 meses |
| 2 | Incentivos (15–20% descuento) para migrar a contratos anuales |
| 3 | Revisión del paquete de Fibra óptica y su propuesta de valor |
| 4 | Descuento mensual por domiciliación automática del pago |
| 5 | Planes familiares para clientes con dependientes |
| 6 | Construcción de modelo predictivo de churn (Random Forest, XGBoost) |

---

## Cómo ejecutar el proyecto

1. Abre el archivo `TelecomX_LATAM.ipynb` en **Google Colab**.
2. Ejecuta las celdas en orden secuencial, una por una ya que google colab lo permite.
3. No se requiere descarga de datos — se cargan automáticamente desde la API con el url que esta al pricipio.

```python
# La extracción de datos ocurre automáticamente en la primera celda de código
url = "https://raw.githubusercontent.com/ingridcristh/challenge2-data-science-LATAM/main/TelecomX_Data.json"
```

**Dependencias necesarias** (disponibles por defecto en Colab):
```
pandas
requests
matplotlib
seaborn
```

---

## Autor

**Jostyn Yesid Reyes**

El proyecto fue desarrollado como parte del desafio de **ONE - Oracle Next Education** en colaboración con **Alura LATAM**.

