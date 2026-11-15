# Ciencia de Datos · Advance UCSC · Proyecto Analítico

**Estudiante:** Fernando Covarrubias Pizarro · **Entrega Final, Secciones A y B (semana 5)**

**Pregunta analítica:** ¿Cómo se asocia el rendimiento promedio (`PROM_ASIS`) con la tasa de retiro (`RET_HOM_TO`, `RET_MUJ_TO`) y la dependencia administrativa (`COD_DEPE`)?

**Datos:** Resumen de Rendimiento 2025, Ministerio de Educación de Chile (Datos Abiertos Mineduc, https://datosabiertos.mineduc.cl), descargado el 18 de octubre de 2026. Licencia: Datos Abiertos Mineduc. `data/mi_conjunto_minado.csv` es el conjunto de trabajo de las semanas 2 a 4: 15.767 filas y 227 columnas; unidad de análisis: establecimiento (RBD) × tipo de enseñanza (COD_ENSE), año 2025. Son datos agregados por establecimiento; no hay datos de personas.

## Cómo reejecutarlo (sin preguntar nada)

Requiere Python 3.13 (probado) y los paquetes de `requirements.txt`, con versiones exactas.

```bash
git clone https://github.com/fcovarrubiasp-debug/CIENCIA-DE-DATO-.git
cd CIENCIA-DE-DATO-
python -m venv .venv
source .venv/bin/activate            # en Windows: .venv\Scripts\activate
pip install -r requirements.txt
cd notebooks
jupyter nbconvert --to notebook --execute --inplace informe_analitico.ipynb
```

Si `jupyter` no se reconoce, usa `python -m nbconvert --to notebook --execute --inplace informe_analitico.ipynb`. El cuaderno corre completo en unos 40 segundos y regenera, sin tocar nada a mano:

| Salida | Contenido |
|---|---|
| `figuras/figura_1.png` | Asistencia promedio por dependencia administrativa |
| `figuras/figura_2.png` | Cinco regiones con mayor asistencia promedio |
| `resultados/resultados.json` | Todas las cifras que cita el informe |

**Semillas:** `20264` (la de la semana 4: submuestras del 80 %, k-means y estabilidad) y `20265` (semana 5: bootstrap por establecimiento).

**Verificación incluida:** el cuaderno empieza con `assert` sobre el conjunto (15.767 × 227, solo 2025, sin nulos en las variables de la pregunta, `RET_TOTAL_TO = RET_HOM_TO + RET_MUJ_TO`, sin duplicados por RBD + COD_ENSE) y sobre la muestra de análisis (14.780 filas, la cifra validada en la semana 4). Si el archivo cambia, falla antes de producir cifras. Con las versiones fijadas, el ρ de Spearman global es −0,157, igual que en la semana 4.

## Flujo del curso (de la semana 1 a la 5)

| Semana | Archivo | Qué hace |
|---|---|---|
| 1 | `2_S1_Notebook_Base_CDD_Fernando_Covarrubias.ipynb` | Pregunta y conjunto base |
| 2 | `7_S2_Notebook_Laboratorio_CDD.ipynb` | Diagnóstico de calidad y flujo de limpieza (`limpiar()`) |
| 3 | `7_S3_Notebook_Laboratorio_CDD_(2).ipynb` | Análisis de gráficos y matriz de correlación |
| 4 | `S4_Entrega3_COVARRUBIAS_FERNANDOok.ipynb` | Segmentación k-means, anomalías y validación |
| 5 | `S5_laboratorio_fernando_covarrubias_CDD.ipynb` | Gráficos explicativos, anonimización, seudonimización y sesgo |
| 5 | `notebooks/informe_analitico.ipynb` | Cuaderno reproducible del informe final |

*Convención:* «celda N» en el informe y la bitácora es la celda N de cada cuaderno contando desde 1, incluidas las de texto.

## Contenido nuevo de esta entrega

| Ruta | Qué es |
|---|---|
| `notebooks/informe_analitico.ipynb` | Verificación, tasa de retiro (definición S4), figuras, asociación con robustez, sensibilidad, segmentación, k-anonimato y verificación de `DEPE_*` |
| `data/mi_conjunto_minado.csv` | Conjunto de trabajo (separador `;`) |
| `figuras/`, `resultados/` | Salidas del cuaderno |
| `informe/` | Informe en Word, bitácora de decisiones (.xlsx) y ficha ético-legal |

## Decisiones clave

- La tasa de retiro es retirados / matrícula (aprobados + reprobados + retirados); se excluyen 7 filas con asistencia 0 y 982 con menos de 10 alumnos (987 en total; quedan 14.780).
- La dependencia se mide con `COD_DEPE2` (1 Municipal, 2 Particular Subvencionado, 3 Particular Pagado, 4 Corporación de Administración Delegada, 5 SLEP). Las columnas `DEPE_*` no se usan: sus nombres están desfasados (el cuaderno lo verifica en la sección 7).
- Correlación de Spearman, con 20 submuestras del 80 % y bootstrap por establecimiento (RBD), porque las filas de un mismo RBD no son independientes.
- Las formulaciones son de **asociación**, no de causalidad.
