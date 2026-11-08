# Laboratorio 12. Turismo en Guatemala con Power BI

El tablero ayuda a entender de dónde vienen los visitantes, por dónde ingresan, en qué meses llegan más y qué tan cerca estuvieron los pronósticos de las cifras registradas.

Trabajamos en **Power BI Service**, la versión que se usa desde el navegador. El equipo trabajó en macOS y Power BI Desktop no tiene una versión nativa para ese sistema. Con la cuenta gratuita que usamos, el equipo se pasó los archivos de Power BI (`.pbix`) por turnos a través de Google Drive.

## Dónde encontrar los archivos

| Ubicación | Qué encontrarás |
| --- | --- |
| [Informe del Laboratorio 12](informe.pdf) | El diseño, las capturas, las pruebas y quién se encargó de cada tarea. |
| [Tablero final en Power BI (v07)](https://drive.google.com/file/d/1UI6BhWENAC5mtbPTC_54_ni3vxdT9rYm/view?usp=drivesdk) | El archivo `.pbix` que se usó para las capturas y las pruebas del informe. |
| [Carpeta compartida en Google Drive](https://drive.google.com/drive/folders/1K2UUz4DVAn33zeMlntpHTkr57oVTi1_v) | El historial del tablero, desde la carga de datos en la v01 hasta la versión final v07. |
| [Datos del tablero](../../src/data/) | Los archivos de datos y los resultados de los modelos que usa el tablero. |
| [Informe del Laboratorio 10](../laboratorio_10/informe.pdf) | La documentación de la versión web, cuyo código está en la raíz del repositorio. |

La v07 añadió la comparación de llegadas por vía aérea entre 2019 y 2025. Usa esa versión para repetir las pruebas del informe.

## Abrir el tablero en Power BI

1. Descarga el archivo `.pbix` de la v07 desde el enlace al tablero final.
2. Entra a [Power BI Service](https://app.powerbi.com/) e inicia sesión. Abre **Mi área de trabajo** o un espacio donde tengas permiso para cargar archivos.
3. Elige **Cargar (Upload)** y luego **Examinar (Browse)**. Selecciona el archivo `.pbix`. Power BI cargará el informe y el conjunto de datos que usan sus gráficos.
4. Abre el informe y ve a **Inicio**. Desde la barra superior puedes entrar a **Mercados de origen**, **Vías de ingreso**, **Temporadas** y **Pronósticos**.

El archivo ya trae los datos. Para consultar el tablero no necesitas descargarlos aparte ni cambiar rutas. [Guía de Microsoft para cargar archivos](https://learn.microsoft.com/en-us/power-bi/connect-data/service-desktop-files).

## De dónde salen los datos y cómo actualizarlos

La carpeta [`src/data/`](../../src/data/) contiene los archivos en formato JSON. Estos son los que se usaron para armar las tablas del tablero.

| Archivo dentro de `src/data/` | Tabla en Power BI |
| --- | --- |
| `general/total-mensual.json` | `total_mensual` |
| `mercados/continentes-anual.json` | `continentes_anual` |
| `temporadas/continentes-mensual.json` | `continentes_mensual` |
| `vias/vias-anual.json` | `vias_anual` |
| `vias/fronteras-anual.json` | `fronteras_anual` |
| `vias/paises-aereos-anual.json` | `paises_aereos_anual` |
| `pronosticos/predicciones.json` | `predicciones` |
| `pronosticos/metricas.json` | `metricas` |

La tabla `predicciones` guarda las llegadas estimadas para cada mes, modelo y serie, como el total nacional. La tabla `metricas` guarda el error de cada modelo para cada serie. El MAE y el RMSE se expresan en llegadas. El MAPE muestra el error promedio en porcentaje.

Para actualizar los datos en una copia de la v07:

1. Abre el modelo de datos del informe en modo **Edición** y selecciona **Transformar datos** para entrar a Power Query.
2. Abre la tabla que quieras actualizar. En el paso **Tabla**, revisa en qué orden aparecen las columnas. Prepara los datos nuevos como filas en ese mismo orden y reemplaza las del paso **Filas**.
3. Si quieres que Power BI lea un archivo externo, modifica el paso **Filas** para usar una ubicación a la que Power BI Service tenga acceso. Conserva los nombres de las tablas y los pasos **Tabla**, **Tipos** y **Fecha** cuando este último aparezca.
4. Guarda y actualiza el modelo. Abre el informe y revisa las fechas, los filtros y las cifras antes de compartir la nueva versión.

[Guía de Microsoft para editar el modelo de datos](https://learn.microsoft.com/en-us/power-bi/transform-model/service-edit-data-models).

## Abrir la versión web del Laboratorio 10

Necesitas Node.js 22.12 o posterior y pnpm 10. Abre una terminal en la carpeta principal del repositorio y ejecuta:

```sh
pnpm install --frozen-lockfile
pnpm dev
```

Abre en el navegador la dirección que indique la terminal, normalmente `http://localhost:3000`.

## Repetir las tres pruebas

Empieza en **Inicio** con el tablero cargado. En cada prueba, cuenta los clics y mide el tiempo desde la primera acción hasta completar la tarea. En la segunda prueba, el tiempo también incluye el regreso a **Inicio**.

| Prueba | Qué hacer | Qué deberías encontrar |
| --- | --- | --- |
| 1. Identificar el mercado principal en 2024 | Abre **Mercados de origen**, selecciona 2024 y revisa la primera barra. En Power BI, pasa el cursor sobre la barra para ver la cifra exacta. | Centroamérica aportó 2.115.335 llegadas, el 64,60 % del total. |
| 2. Consultar el cambio en Pedro de Alvarado | Abre **Vías de ingreso** y selecciona la vía terrestre. Busca Pedro de Alvarado para comparar 2009 con 2025. Ese período ya está fijado en Power BI. Regresa a **Inicio**. | Las llegadas aumentaron 875,29 %. La web lo redondea a 875,3 %. |
| 3. Comparar los pronósticos del total nacional | Abre **Pronósticos**, elige **Total nacional** y busca abril de 2021. En la web, cambia de modelo para leer cada estimación. En Power BI, pasa el cursor por el punto del mes para ver el detalle. Después compara el error porcentual (MAPE) de los tres modelos en todo el período de prueba. | En abril se registraron 59.743 llegadas. El suavizamiento exponencial simple estimó 85.017, SARIMA estimó 35.581 y la red LSTM estimó 89.553. En el período completo, la LSTM tuvo el menor MAPE, con 32,8 %. |

Las figuras 14 a 20 del [informe final](informe.pdf) muestran los resultados de las pruebas. Las figuras 9 a 13 muestran las cinco páginas de la v07.
