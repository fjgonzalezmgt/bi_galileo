# 📊 Proyecto BI - Análisis de Netflix

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-D71F00?style=for-the-badge&logo=sqlalchemy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)

## 📖 Descripción

Proyecto de Business Intelligence & Analytics desarrollado para la **Universidad Galileo**, enfocado en el análisis de datos de películas y series de Netflix. Este proyecto implementa un pipeline completo de ETL (Extract, Transform, Load) utilizando Python y tecnologías modernas de análisis de datos.

El proyecto incluye:
- 🔄 Proceso ETL automatizado con Python
- 📊 Análisis exploratorio de datos (EDA)
- 🗄️ Modelo de base de datos en esquema estrella
- 📈 Dashboard interactivo en Power BI
- 🐘 Base de datos PostgreSQL

## 🎯 Características

- **Extracción de datos**: Lectura y procesamiento de archivos CSV con datos de Netflix
- **Transformación de datos**: 
  - Normalización de fechas
  - División de columnas de duración en cantidad y tipo
  - Expansión de datos multi-valorados (géneros, elenco, países)
  - Manejo de valores nulos
- **Esquema Estrella**: Implementación de modelo dimensional con:
  - Tabla de hechos (fact_table)
  - 5 Tablas dimensionales (título, personas, país, fecha, tipo)
- **Carga a PostgreSQL**: Inserción automática de datos transformados
- **Análisis Visual**: Dashboard interactivo en Power BI

## 🗂️ Estructura del Proyecto

```
bi_galileo/
├── EDA.ipynb                  # Análisis exploratorio de datos
├── ETL Proyecto.ipynb         # Notebook del proceso ETL
├── proyecto_library.py        # Biblioteca con funciones ETL
├── db_scripts.sql            # Scripts SQL para constraints de BD
├── tablero_netflix.pbix      # Dashboard de Power BI
├── EDA Access.md             # Enlace al EDA publicado
├── requirements.txt          # Dependencias de Python
├── .gitignore               # Archivos a ignorar en git
└── LICENSE.txt               # Licencia del proyecto
```

## 📋 Requisitos Previos

- Python 3.8 o superior
- PostgreSQL 12 o superior
- Power BI Desktop (para visualizar el dashboard)
- Jupyter Notebook o JupyterLab

## 🔧 Instalación

1. **Clonar el repositorio**
```bash
git clone https://github.com/fjgonzalezmgt/bi_galileo.git
cd bi_galileo
```

2. **Instalar dependencias de Python**
```bash
pip install pandas>=1.5.0 sqlalchemy>=1.4.0 numpy>=1.23.0 ydata-profiling>=4.0.0 openpyxl>=3.0.0 psycopg2-binary>=2.9.0
```

O usando un archivo de requirements:
```bash
pip install -r requirements.txt
```

3. **Configurar la base de datos PostgreSQL**
   - Crear una base de datos en PostgreSQL
   - Crear un archivo `credenciales_bd.xlsx` con las siguientes columnas:
     - tipo: user, clave, host, puerto, db_name
     - descripcion: [tus valores correspondientes]
   
   > ⚠️ **Nota de Seguridad**: Para entornos de producción, se recomienda usar variables de entorno o sistemas de gestión de secretos (como AWS Secrets Manager, Azure Key Vault, o HashiCorp Vault) en lugar de archivos Excel. El archivo de credenciales debe estar incluido en `.gitignore` para evitar exponer información sensible.

4. **Preparar los datos**
   - Asegurarse de tener el archivo `netflix_titles.csv` en el directorio raíz

## 🚀 Uso

### 1. Análisis Exploratorio de Datos (EDA)

```bash
jupyter notebook "EDA.ipynb"
```

Este notebook genera un reporte completo de análisis exploratorio de los datos de Netflix.

🔗 **Ver EDA online**: [EDA Películas Netflix](https://qualityanalytics.net/wp-content/uploads/2025/06/EDA_peliculas_netflix.html)

### 2. Proceso ETL

```bash
jupyter notebook "ETL Proyecto.ipynb"
```

Este notebook ejecuta el proceso completo de ETL:
1. Extrae datos del archivo CSV
2. Transforma los datos al modelo de esquema estrella
3. Carga los datos a PostgreSQL

### 3. Uso de la Biblioteca ETL

```python
from proyecto_library import read_file, create_star_schema, initialize_db, load_data_to_table

# Leer archivo
data = read_file('netflix_titles.csv')

# Crear esquema estrella
star_schema = create_star_schema(data)

# Inicializar base de datos
engine = initialize_db(username, password, host, port, database_name)

# Cargar datos
for tabla, dataframe in star_schema.items():
    load_data_to_table(engine, tabla, dataframe)
```

## 🗄️ Esquema de Base de Datos

### Tabla de Hechos
- **fact_table**: Contiene las métricas principales y claves foráneas a las dimensiones

### Tablas Dimensionales
1. **title_dim**: Información de títulos y géneros
2. **people_dim**: Directores y elenco
3. **country_dim**: Países de producción
4. **date_dim**: Fechas de adición a Netflix
5. **type_dim**: Tipos de contenido, ratings y duración

### Relaciones
Todas las tablas dimensionales están relacionadas con la tabla de hechos mediante claves foráneas (foreign keys) configuradas en `db_scripts.sql`.

## 📊 Dashboard Power BI

El archivo `tablero_netflix.pbix` contiene visualizaciones interactivas que incluyen:
- Distribución de contenido por tipo (películas vs series)
- Análisis temporal de adiciones a la plataforma
- Top países productores
- Análisis de ratings y géneros
- Métricas de duración

Para abrir el dashboard:
```bash
# Abrir con Power BI Desktop
tablero_netflix.pbix
```

## 🛠️ Funciones Principales

### `read_file(file_path)`
Lee y transforma el archivo CSV de Netflix, realizando:
- Conversión de fechas
- División de columnas multi-valor
- Manejo de valores nulos

### `create_star_schema(df)`
Transforma un DataFrame en un modelo de esquema estrella completo con tablas de hechos y dimensiones.

### `initialize_db(username, password, host, port, database_name)`
Inicializa la conexión a la base de datos PostgreSQL.

### `load_data_to_table(engine, table_name, data, if_exists='replace')`
Carga un DataFrame a una tabla específica en PostgreSQL.

## 👨‍💼 Autor

**Fernando José González**
- GitHub: [@fjgonzalezmgt](https://github.com/fjgonzalezmgt)
- Proyecto: Universidad Galileo - Maestría en Business Intelligence & Analytics

## 📄 Licencia

Este proyecto está bajo la Licencia Apache 2.0. Ver el archivo [LICENSE.txt](LICENSE.txt) para más detalles.

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor:
1. Fork el proyecto
2. Crea una rama para tu característica (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📧 Contacto

Para preguntas o sugerencias sobre este proyecto, por favor abre un issue en el repositorio de GitHub.

---

⭐ Si este proyecto te fue útil, considera darle una estrella en GitHub!
