# 📊 Proyecto 3: Análisis de Residuos y Economía Circular

> **Data Analysis Portfolio** | México en Contexto Internacional

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![SQL](https://img.shields.io/badge/SQL-SQLite-orange.svg)](https://www.sqlite.org/)
[![Plotly](https://img.shields.io/badge/Plotly-5.0+-green.svg)](https://plotly.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 🎯 Resumen Ejecutivo

Análisis cuantitativo de **gestión de residuos y economía circular** comparando México con 18 países (13 europeos, 5 latinoamericanos). 

**Hallazgo principal:** México recicla 9.3% vs Polonia 35.5% (benchmark realista). Polonia mejoró **6.6× más rápido** que México aplicando políticas específicas que SÍ son replicables.

**Deliverables:**
- 📊 14 visualizaciones interactivas
- 🗄️ Base de datos SQL (7 tablas, ~200 registros)
- 📄 Informe de 5 recomendaciones empresariales (estructura ISO 14001)
- 💻 4 queries SQL avanzados (CTEs, window functions)

---

## 🚀 Visualizaciones Destacadas

### Dashboard 1: Contexto y Problema
[![Dashboard 1](https://img.shields.io/badge/Ver-Dashboard%201-blue)](https://laura8am.github.io/portfolio-data-analyst/dashboard1_contexto.html)

**Paneles:**
1. Tasas de reciclaje — Top 10 países
2. Evolución histórica 2010-2022 (Polonia vs México)
3. Gap vs Polonia (benchmark realista)
4. Composición de residuos: México vs Europa

### Dashboard 2: Análisis y Oportunidades
[![Dashboard 2](https://img.shields.io/badge/Ver-Dashboard%202-green)](https://laura8am.github.io/portfolio-data-analyst/dashboard2_oportunidades.html)

**Paneles:**
5. Matriz de oportunidades empresariales
6. Correlación PIB per cápita vs Reciclaje (r=0.87)
7. Velocidades de mejora comparadas

### Mapas Geográficos
- 🗺️ [Mapa Europa: Tasas de Reciclaje](https://laura8am.github.io/portfolio-data-analyst/mapa_europa_reciclaje.html)
- 🗺️ [Mapa Europa: Generación per cápita](https://laura8am.github.io/portfolio-data-analyst/mapa_europa_generacion.html)

### Otras Visualizaciones
- 📊 [Gap Analysis (tabla semáforo)](https://laura8am.github.io/portfolio-data-analyst/sesion5_gap_semaforo.html)
- 📊 [Matriz de Oportunidades](https://laura8am.github.io/portfolio-data-analyst/viz6_matriz_oportunidades.html)
- 📈 [Heatmap de Correlaciones](viz7_heatmap_correlaciones.png)
- 📈 [Evolución Histórica 2010-2022](viz5_evolucion_historica.png)

---

## 🔑 Hallazgos Clave

### 1. Gap de Reciclaje
- **México:** 9.3% reciclaje vs 84% disposición inadecuada
- **Polonia:** 35.5% reciclaje vs 32% disposición inadecuada
- **Gap:** 26.2 puntos porcentuales

### 2. Velocidades de Mejora
- **Polonia:** +2.36 puntos/año (2010-2022)
- **México:** +0.36 puntos/año (2010-2022)
- **Diferencia:** Polonia mejoró **6.6× más rápido**

### 3. Oportunidad Principal: Orgánicos
- **Volumen:** 28.27 millones ton/año (52.4% de residuos)
- **Aprovechamiento actual:** 2%
- **Desperdicio:** 98% va a relleno sanitario
- **Recomendación:** Planta de compostaje industrial (Quick Win)

### 4. Correlación PIB-Reciclaje... PERO
- **r = 0.87** (correlación fuerte)
- **PERO:** Polonia (PIB $18k) recicla 35.5%, México (PIB $10k) recicla 9.3%
- **Conclusión:** Las **políticas públicas** determinan más que el PIB

### 5. Patrón Geográfico Europa
- **Occidental:** 56.0% reciclaje promedio
- **Norte:** 46.4% (alto consumo, buena gestión)
- **Sur:** 33.3% (en transición)
- **Este:** 27.1% (mejorando rápido)

---

## 🛠️ Stack Técnico

**Lenguajes y Herramientas:**
- ![Python](https://img.shields.io/badge/-Python-3776AB?style=flat&logo=python&logoColor=white) Python 3.8+ (pandas, numpy)
- ![SQL](https://img.shields.io/badge/-SQL-4479A1?style=flat&logo=sqlite&logoColor=white) SQLite (queries avanzados: CTEs, window functions)
- ![Plotly](https://img.shields.io/badge/-Plotly-3F4F75?style=flat&logo=plotly&logoColor=white) Plotly (dashboards interactivos)
- ![Seaborn](https://img.shields.io/badge/-Seaborn-4C72B0?style=flat) Seaborn (visualizaciones estáticas)

**Ambiente:**
- Google Colab (desarrollo)
- GitHub Pages (publicación)
- Notion (documentación)

---

## 📁 Estructura del Proyecto
```
portfolio-data-analyst/
│
├── README.md                          # Este archivo
├── waste_management.db                # Base de datos SQLite (7 tablas)
│
├── dashboards/
│   ├── dashboard1_contexto.html
│   └── dashboard2_oportunidades.html
│
├── mapas/
│   ├── mapa_europa_reciclaje.html
│   └── mapa_europa_generacion.html
│
├── visualizaciones/
│   ├── viz1_ranking_reciclaje.png
│   ├── viz5_evolucion_historica.png
│   ├── viz7_heatmap_correlaciones.png
│   └── ... (14 visualizaciones totales)
│
├── queries/
│   ├── query1_caso_polonia.sql
│   ├── query2_trayectoria.sql
│   ├── query3_ranking.sql
│   └── query4_analisis_integral.sql
│
└── documentos/
    ├── informe_recomendaciones.txt    # 5 recomendaciones ISO-style
    └── sesion13_sql_avanzado_doc.txt
```

---

## 💼 Las 5 Recomendaciones Empresariales

**Estructura ISO 14001:** Objetivo + KPI + Plazo + Fundamentación en datos

1. **🌱 Compostaje Industrial** — Quick Win
   - Inversión: $5-10M MXN | ROI: 3.5:1 en 2-4 años
   - Target: 50,000 ton/año de orgánicos

2. **⚡ Modelo Polonia** — Mejora Acelerada
   - Acelerar de 0.36 a 1.5 pts/año
   - Meta 2030: 25% reciclaje

3. **🎯 Focalización Geográfica** — Implementación Secuencial
   - Fase 1: 10 zonas metro (20% población)
   - Fase 2: 30 ciudades medias (35% población)

4. **📊 Sistema de Monitoreo** — Transparencia
   - Dashboard público mensual
   - Inversión: $54M MXN (3 años)

5. **♻️ Separación en Fuente** — Cambio Cultural
   - Meta: 60% hogares separando en 5 años
   - Educación + Infraestructura + Consecuencias

**Inversión total 10 años:** ~$34,300M MXN | **ROI:** 2.3:1

---

## 📊 Datos y Metodología

**Fuentes de datos:**
- EUROSTAT 2022 (Municipal Waste Statistics)
- BID 2022 (América Latina y el Caribe)
- SEMARNAT 2021 (México)
- Valores de mercado México 2022

**Base de datos:**
- 7 tablas SQL relacionales
- ~200 registros
- 19 países analizados

**Cobertura:**
- 🇪🇺 Europa: 13 países
- 🌎 América Latina: 5 países
- 🇲🇽 México: datos completos

**Decisiones metodológicas:**
- ✅ Sin proyecciones débiles (muestra pequeña)
- ✅ Análisis descriptivo riguroso
- ✅ Fuentes documentadas en todas las visualizaciones
- ✅ Limitaciones reconocidas explícitamente

---

## 🎓 Progresión de Habilidades SQL

**Sesión 1-2:** SELECT, WHERE, ORDER BY, GROUP BY básico
**Sesión 3-5:** JOINs múltiples, CASE WHEN, subqueries
**Sesión 6-9:** Window functions, correlaciones, LEFT JOIN avanzado
**Sesión 13:** CTEs, LAG(), ROW_NUMBER(), subqueries correlacionadas

**Query más complejo:** Análisis integral con 4 CTEs encadenados + subqueries

---

## 👤 Sobre Mí

**Laura Ochoa M.**  
Data Analyst | Especialista en Sistemas de Gestión (ISO 9001/14001/45001)

**Experiencia:**
- 2 años gestionando ISO 14001 en sector residuos
- Conocimiento práctico de operaciones de reciclaje
- Análisis de datos aplicado a sostenibilidad

**Conecta conmigo:**
- 💼 [LinkedIn](https://linkedin.com/in/lauraochoam)
- 🐙 [GitHub](https://github.com/laura8am)

---

## 📜 Licencia

Este proyecto está bajo licencia MIT. Ver archivo `LICENSE` para más detalles.

---

## 🙏 Agradecimientos

- EUROSTAT por datos públicos de alta calidad
- SEMARNAT por estadísticas de México
- Comunidad de Plotly por documentación excelente

---

**⭐ Si este proyecto te resultó útil, considera darle una estrella en GitHub**

*Última actualización: Marzo 2026*
