# ⚖️ Dashboard CSJJ Junín — *Tukuy Rikuy*

> *"Tukuy Rikuy"* (quechua: **el que todo lo ve**) — Aplicativo de inteligencia analítica para la **Corte Superior de Justicia de Junín**.

Plataforma interactiva construida en **Streamlit** que transforma data judicial bruta en visualizaciones geoespaciales y estadísticas accionables sobre carga procesal, audiencias, personal jurisdiccional y proyecciones demográficas de los 9 distritos judiciales de Junín.

---

## 🧭 ¿Qué hace?

El dashboard ingiere un único archivo Excel multi-hoja (`data/data.xlsx`) y lo renderiza en **5 módulos analíticos**:

| Tab | Módulo | Descripción |
|----|--------|-------------|
| 📈 | **Proyecciones Poblacionales** | Crecimiento poblacional INEI 2017→2026, ratio jueces/habitantes y semáforo de cobertura jurisdiccional. |
| 🏛️ | **Resumen & Detalle Provincia** | Choropleths interactivos (distrital y provincial) de Junín sobre `GeoJSON` con etiquetas centradas por **centroide planar** ponderado. |
| 📂 | **Carga Procesal** | Evolución multi-anual de Ingresos / Carga / Resueltos por especialidad (Civil, Penal, Familia, Laboral, Constitucional, Extinción de Dominio). |
| 🎙️ | **Audiencias** | KPIs de programadas vs. realizadas vs. suspendidas, con tasa de atención por sede y análisis SNEJ por juzgado. |
| 👥 | **Personal Jurisdiccional** | Composición por régimen laboral (Ley 29277, D.L. 728, D.L. 276, CAS, RECAS), género, condición (titular/supernumerario/provisional) y cargos. |

---

## 🛠️ Stack técnico

```
Python 3.x
├── streamlit            # capa de UI reactiva
├── pandas + openpyxl    # ETL desde Excel (10+ hojas: Carga_Procesal, Pob_Proyeccion,
│                          Jueces_Pob_2026, Personal_Regimen, Audiencias_Data,
│                          Resumen/SNEJ, Alerta_Sobrecarga, ...)
├── plotly.express/go    # gráficos interactivos (barras, líneas, treemaps, sunburst)
├── folium + streamlit-  # mapas coropléticos sobre GeoJSON Perú
│   folium                 (distrital ~1.8 MB, provincial ~800 KB)
└── xlsxwriter           # exportación de reportes
```

### Detalles de ingeniería destacables

- **Cacheo invalidado por hash del Excel** (`mtime + size`) vía `@st.cache_data` → recarga reactiva sin reiniciar.
- **Geometría computacional propia**: cálculo de centroides por *signed area* de anillos para posicionar etiquetas dentro de polígonos irregulares (no usa el promedio ingenuo de vértices).
- **Theming custom**: plantilla Plotly `custom_theme` con fondo crema institucional + fuente *Andina* embebida en **base64** dentro del CSS para portabilidad total.
- **CSS-in-Streamlit avanzado**: chips de `multiselect` coloreados dinámicamente por especialidad usando selectores `[data-baseweb="tag"]:has(...)`.
- **Parsing defensivo**: helper `_safe_int()` y resolución por nombres de columna alternos para tolerar variaciones en el Excel fuente.

---

## 🚀 Ejecución

```bash
pip install -r requirements.txt
streamlit run app.py
```

La app abre en `http://localhost:8501`. El botón **🔄 Recargar Excel** del sidebar limpia el caché tras editar `data/data.xlsx`.

---

## 📁 Estructura

```
.
├── app.py                  # aplicación Streamlit (monolito declarativo, ~2150 LOC)
├── data/
│   ├── data.xlsx           # fuente única de verdad
│   └── peru_*_simple.geojson
├── assets/
│   ├── fonts/              # tipografía institucional Andina
│   └── images/             # logo, escudos, iconografía
├── scripts/                # utilitarios de validación e inyección de hojas
└── requirements.txt
```

---

**Presidente:** Dr. Ricardo Corrales Melgarejo · **Gestión:** 2025–2026
*Justicia pronta, honesta e inclusiva.*
