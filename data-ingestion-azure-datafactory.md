# Pipeline de Ingesta en Azure Data Factory

Pipeline de datos construido en Azure Data Factory (ADF) que mueve archivos entre zonas de un Data Lake, usando Copy Data activities y ejecución programada mediante triggers. Proyecto enfocado en demostrar dominio práctico de ADF: linked services, datasets, pipelines y automatización.

## 🏗️ Arquitectura

Azure Data Lake Storage Gen2 (container "raw")
│
▼
ADF Pipeline (Copy Data activity)
│
▼
Azure Data Lake Storage Gen2 (container "processed")


## 🔧 Componentes

- **Linked Service:** conexión hacia Azure Data Lake Storage Gen2, autenticada vía Account Key.
- **Datasets:** uno para el origen (`raw`), otro para el destino (`processed`).
- **Pipeline:** actividad Copy Data que mueve el archivo de origen a destino.
- **Trigger:** Schedule Trigger configurado para ejecución automática diaria.

## 📸 Evidencia de ejecución

| Captura | Descripción |
|---|---|
| `screenshots/01_pipeline_canvas.png` | Pipeline completo en el canvas de ADF |
| `screenshots/02_source_sink_config.png` | Configuración de Source y Sink |
| `screenshots/03_pipeline_run_succeeded.png` | Ejecución exitosa en Monitor (Pipeline runs) |
| `screenshots/04_trigger_run_succeeded.png` | Ejecución automática vía trigger (Trigger runs) |
| `screenshots/05_processed_container.png` | Archivo resultante en el container `processed` |

## 📦 Infrastructure as Code

La carpeta `arm-template/` contiene la plantilla ARM exportada directamente desde ADF, con la definición completa de los recursos (pipeline, datasets, linked service, trigger). Las credenciales están excluidas como `secureString` parameters, sin valores expuestos.

## 🔧 Stack técnico

- Azure Data Factory (V2)
- Azure Data Lake Storage Gen2
- ARM Templates (Infrastructure as Code)

## 🚀 Cómo reproducirlo

1. Crea una cuenta de Azure y un recurso de Data Factory.
2. Crea una cuenta de Storage con jerarquía de namespace habilitada (ADLS Gen2).
3. Despliega la plantilla ARM en `arm-template/`, o recrea manualmente el Linked Service, Datasets y Pipeline siguiendo la estructura descrita arriba.
4. Sube un archivo al container `raw` y ejecuta el pipeline en modo Debug, o espera a que el trigger programado lo dispare automáticamente.

## 📌 Notas

- El proyecto usa el tier "siempre gratis" de Azure Data Factory (5 actividades de baja frecuencia al mes), por lo que el trigger se mantiene deshabilitado fuera de las pruebas para no exceder ese límite.
- Detalle aprendido en el proceso: los Schedule Triggers de ADF requieren la hora en formato 24 horas, y no ejecutan retroactivamente si la hora configurada ya pasó al momento de publicarse — esperan a la siguiente ocurrencia programada.