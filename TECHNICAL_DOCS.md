# 📚 Documentación Técnica — Proyecto Residuos y Economía Circular

> Guía técnica para desarrolladores y analistas de datos

---

## 🗄️ Esquema de Base de Datos

### Diagrama Entidad-Relación (Textual)
```
paises (1) ──── (N) generacion_residuos
   │
   ├──── (N) indicadores_ec
   │
   ├──── (N) disposicion_residuos
   │
   ├──── (N) composicion_residuos
   │
   └──── (1) empresa_referencia

valores_mercado (tabla independiente, lookup por tipo_residuo)
```

### Tabla 1: `paises`
**Propósito:** Catálogo maestro de países analizados

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| pais_id | INTEGER PRIMARY KEY | ID único | 19 |
| nombre | TEXT | Nombre del país | 'México' |
| region | TEXT | Región geográfica | 'América Latina' |
| subregion | TEXT | Subregión (solo Europa) | NULL |
| codigo_iso | TEXT | Código ISO 3 letras | 'MEX' |
| pib_per_capita | INTEGER | PIB per cápita USD | 10046 |
| poblacion_m | REAL | Población en millones | 130.0 |

**Registros:** 19 (13 Europa + 5 LAC + 1 México)

---

### Tabla 2: `generacion_residuos`
**Propósito:** Generación de residuos per cápita por país

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| generacion_id | INTEGER PRIMARY KEY | ID único | 1 |
| pais_id | INTEGER FK | Referencia a paises | 19 |
| kg_per_capita | REAL | kg/habitante/año | 415.0 |

**Registros:** 19

---

### Tabla 3: `indicadores_ec`
**Propósito:** Indicadores de economía circular

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| indicador_id | INTEGER PRIMARY KEY | ID único | 1 |
| pais_id | INTEGER FK | Referencia a paises | 19 |
| tasa_reciclaje_pct | REAL | % reciclaje | 9.3 |
| tasa_recuperacion_pct | REAL | % recuperación total | 11.0 |

**Registros:** 19

---

### Tabla 4: `disposicion_residuos`
**Propósito:** Métodos de disposición por país

| Campo | Tipo | Descripción | Valores posibles |
|-------|------|-------------|------------------|
| disposicion_id | INTEGER PRIMARY KEY | ID único | — |
| pais_id | INTEGER FK | Referencia a paises | — |
| operacion | TEXT | Tipo de disposición | 'reciclaje', 'compostaje', 'incineracion', 'relleno_sanitario', 'disposicion_inadecuada' |
| porcentaje | REAL | % del total | 9.3 |

**Registros:** ~95 (5 operaciones × 19 países)

**Restricción:** SUM(porcentaje) por pais_id ≈ 100%

---

### Tabla 5: `composicion_residuos`
**Propósito:** Composición de residuos por tipo y país

| Campo | Tipo | Descripción | Valores posibles |
|-------|------|-------------|------------------|
| composicion_id | INTEGER PRIMARY KEY | ID único | — |
| pais_id | INTEGER FK | Referencia a paises | — |
| tipo_residuo | TEXT | Categoría de residuo | 'orgánico', 'plástico', 'papel_carton', 'vidrio', 'metal', 'otros' |
| porcentaje | REAL | % del total | 52.4 |

**Registros:** 30 (6 tipos × 5 países con datos completos)

**Países con datos de composición:** México, Polonia, Alemania, España, Francia

**Restricción:** SUM(porcentaje) por pais_id ≈ 100%

---

### Tabla 6: `empresa_referencia`
**Propósito:** Empresas gestoras de referencia por país

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| empresa_id | INTEGER PRIMARY KEY | ID único | 1 |
| pais_id | INTEGER FK | Referencia a paises | 19 |
| nombre_empresa | TEXT | Razón social | 'ECOCE' |
| tipo_gestion | TEXT | Especialidad | 'Plásticos' |

**Registros:** 6-8 empresas

---

### Tabla 7: `valores_mercado`
**Propósito:** Precios de mercado por tipo de residuo (México 2022)

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| tipo_residuo | TEXT PRIMARY KEY | Tipo de material | 'plástico' |
| valor_por_ton_mxn | REAL | Precio MXN/tonelada | 8000.0 |
| viabilidad_tecnica | TEXT | Nivel de viabilidad | 'Alta', 'Media', 'Baja' |

**Registros:** 6

**Fuente:** Precios de mercado México 2022 + evaluación técnica propia

---

## 🔧 Instalación y Configuración

### Requisitos Previos
```bash
Python >= 3.8
pip >= 21.0
```

### Instalación de Dependencias
```bash
pip install pandas numpy sqlite3 plotly seaborn matplotlib
```

**Versiones específicas:**
```
pandas==2.0.3
plotly==5.17.0
seaborn==0.12.2
matplotlib==3.7.2
```

### Configuración de Base de Datos

**Opción 1: Descargar DB existente**
```python
# La base de datos ya está construida
import sqlite3
conn = sqlite3.connect('waste_management.db')
```

**Opción 2: Reconstruir desde cero**
```python
# Ejecutar el script de construcción (Sesión 2)
# Ver: notebooks/sesion2_construccion_bd.ipynb
```

---

## 📊 Guía de Uso

### Query Básico: Consultar Datos
```python
import pandas as pd
import sqlite3

# Conectar
conn = sqlite3.connect('waste_management.db')

# Query simple
query = '''
SELECT p.nombre, i.tasa_reciclaje_pct
FROM paises p
INNER JOIN indicadores_ec i ON p.pais_id = i.pais_id
ORDER BY i.tasa_reciclaje_pct DESC
'''

df = pd.read_sql_query(query, conn)
print(df)
```

### Query Avanzado: Análisis con CTEs
```python
query_complejo = '''
WITH datos_base AS (
    SELECT
        p.nombre,
        i.tasa_reciclaje_pct,
        g.kg_per_capita
    FROM paises p
    INNER JOIN indicadores_ec i ON p.pais_id = i.pais_id
    INNER JOIN generacion_residuos g ON p.pais_id = g.pais_id
)
SELECT
    nombre,
    tasa_reciclaje_pct,
    kg_per_capita,
    ROUND(kg_per_capita * tasa_reciclaje_pct / 100, 1) AS kg_reciclados
FROM datos_base
ORDER BY kg_reciclados DESC;
'''

df = pd.read_sql_query(query_complejo, conn)
```

### Crear Visualización
```python
import plotly.express as px

# Preparar datos
query = "SELECT nombre, tasa_reciclaje_pct FROM paises p INNER JOIN indicadores_ec i ON p.pais_id = i.pais_id"
df = pd.read_sql_query(query, conn)

# Visualizar
fig = px.bar(df, x='nombre', y='tasa_reciclaje_pct',
             title='Tasas de Reciclaje por País')
fig.show()
```

---

## 🧠 Decisiones Técnicas Importantes

### 1. ¿Por qué SQLite y no PostgreSQL/MySQL?

**Decisión:** SQLite

**Razones:**
- Portabilidad: Un solo archivo `.db`
- Sin servidor: Fácil compartir en GitHub
- Suficiente para dataset pequeño (<1MB)
- Soporta CTEs y window functions (necesarios)

**Trade-off:** No soporta múltiples escritores concurrentes (no es problema para análisis)

---

### 2. ¿Por qué Plotly y no solo Matplotlib?

**Decisión:** Plotly para dashboards, Seaborn para estáticos

**Razones:**
- Plotly: Interactividad sin código JavaScript
- Seaborn: Mejor para heatmaps y visualizaciones estáticas profesionales
- Ambos: Diferentes use cases

**Trade-off:** Dos librerías en lugar de una (peso mayor)

---

### 3. ¿Por qué NO proyecciones futuras?

**Decisión:** Solo análisis descriptivo histórico

**Razones:**
- Muestra pequeña (7 puntos temporales por país)
- R² insuficiente para proyecciones confiables
- Integridad analítica > Completar todos los entregables

**Lección:** Mejor reconocer limitaciones que presentar análisis débil

---

### 4. ¿Por qué NO mapa de México por estado?

**Decisión:** Solo mapas de Europa

**Razones:**
- SEMARNAT no publica datos estatales confiables
- Metodologías inconsistentes entre entidades
- Preferimos NO visualizar antes que visualizar datos poco confiables

**Alternativa:** Mapas de Europa donde SÍ hay datos de calidad (EUROSTAT)

---

### 5. Estructura de IDs: ¿Auto-increment o manual?

**Decisión:** Auto-increment para _id, manual para pais_id

**Razones:**
- `pais_id` manual (1-19) facilita debugging
- `generacion_id`, etc. auto-increment (menos error humano)

**Ejemplo:**
```sql
CREATE TABLE paises (
    pais_id INTEGER PRIMARY KEY,  -- Manual
    nombre TEXT
);

CREATE TABLE generacion_residuos (
    generacion_id INTEGER PRIMARY KEY AUTOINCREMENT,  -- Auto
    pais_id INTEGER,
    FOREIGN KEY (pais_id) REFERENCES paises(pais_id)
);
```

---

## 🐛 Problemas Comunes y Soluciones

### Problema 1: "no such table: paises"

**Causa:** Base de datos no creada o conexión incorrecta

**Solución:**
```python
# Verificar que el archivo existe
import os
print(os.path.exists('waste_management.db'))  # Debe ser True

# Reconectar
conn = sqlite3.connect('waste_management.db')
```

---

### Problema 2: Datos de México aparecen en Alemania

**Causa:** México tiene `pais_id = 19`, no `pais_id = 1`

**Solución:**
```python
# Verificar pais_id correcto
query = "SELECT pais_id, nombre FROM paises WHERE nombre = 'México'"
print(pd.read_sql_query(query, conn))
# Debe mostrar: 19, 'México'
```

---

### Problema 3: Visualizaciones no se guardan

**Causa:** `plt.savefig()` después de `plt.show()`

**Solución:**
```python
# CORRECTO: savefig ANTES de show
plt.savefig('grafica.png')
plt.show()

# INCORRECTO:
plt.show()
plt.savefig('grafica.png')  # Se guarda vacío
```

---

### Problema 4: Fuentes encimadas en gráficas

**Causa:** Espacio insuficiente en margen inferior

**Solución:**
```python
# Para Seaborn/Matplotlib
plt.subplots_adjust(bottom=0.15)  # Más espacio abajo

# Para Plotly
fig.update_layout(margin=dict(b=150))
```

---

## 📏 Convenciones de Código

### Nombres de Variables
```python
# BIEN: Descriptivos y snake_case
df_correlaciones = pd.read_sql_query(query, conn)
tasa_reciclaje_mexico = 9.3

# MAL: Crípticos o camelCase
df1 = pd.read_sql_query(query, conn)
tasaRecMex = 9.3
```

### Nombres de Archivos
```
# BIEN: Prefijo de sesión + descripción
sesion5_gap_semaforo.html
viz7_heatmap_correlaciones.png

# MAL: Sin contexto
grafica1.png
dashboard.html
```

### Queries SQL
```sql
-- BIEN: CTEs con nombres descriptivos
WITH polonia_actual AS (
    SELECT nombre, tasa_reciclaje_pct
    FROM paises p
    INNER JOIN indicadores_ec i ON p.pais_id = i.pais_id
    WHERE nombre = ' Polonia'
)

-- MAL: Subqueries anidados sin nombres
SELECT * FROM (
    SELECT * FROM (
        SELECT ...
    )
)
```

---

## 🧪 Testing y Validación

### Validación de Integridad de Datos
```python
# Test 1: Suma de composición = 100%
query = '''
SELECT pais_id, SUM(porcentaje) AS total
FROM composicion_residuos
GROUP BY pais_id
'''
df = pd.read_sql_query(query, conn)
assert all(df['total'].between(99, 101)), "Composición no suma 100%"

# Test 2: No hay valores NULL en campos críticos
query = "SELECT COUNT(*) FROM paises WHERE nombre IS NULL"
assert pd.read_sql_query(query, conn).iloc[0,0] == 0, "Nombres NULL encontrados"
```

---

## 📦 Archivos de Salida

### Visualizaciones HTML (Plotly)
- Tamaño: 300KB - 2MB por archivo
- Formato: HTML autónomo (self-contained)
- Dependencias: Plotly.js embebido

### Visualizaciones PNG (Seaborn)
- Tamaño: 50-200KB por archivo
- Resolución: 150 DPI
- Formato: PNG con transparencia

### Base de Datos
- Tamaño: ~500KB
- Formato: SQLite 3
- Compresión: No aplicada (no necesaria)

---

## 🔄 Flujo de Trabajo Típico
```
1. Conectar a BD
   ↓
2. Diseñar query SQL
   ↓
3. Ejecutar y validar resultados
   ↓
4. Crear visualización
   ↓
5. Exportar (HTML o PNG)
   ↓
6. Subir a GitHub
   ↓
7. Verificar en GitHub Pages
   ↓
8. Embed en Notion
```

---

## 📞 Soporte

**Para preguntas técnicas:**
- Abrir issue en GitHub
- LinkedIn: [Laura Ochoa M.](https://linkedin.com/in/lauraochoam)

**Para colaboraciones:**
- Fork el proyecto
- Enviar Pull Request con descripción clara

---

*Última actualización: Marzo 2026*
*Versión: 1.0*
