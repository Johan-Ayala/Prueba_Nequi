# Modelo de probabilidad de default (PD)

Modelo que estima, al momento de la solicitud, la probabilidad de que un cliente llegue a **90 días de mora en sus primeros 4 meses** de crédito.

## Archivos

| Archivo | Contenido |
|---|---|
| `revision variables.py` | Pipeline principal: unión variables y predictora, revisión de variables, modelo, KS/AUC y PSI |
| `generacion_y.py` | Análisis exploratorio de la cartera para definir la Y |
| `Preguntas.docx` | Detalle completo del análisis y los resultados |
| `Resumen ejecutivo.docx` | Informe para los stakeholders |
| `respuesta.csv` | Variable Y guardada y consolidada |

## Cómo ejecutar

```bash
pip install pandas numpy scikit-learn
python modelo_pd.py
```

Las rutas de los CSV se configuran al inicio del script (`RUTA_X`, `RUTA_CARTERA`).

## Definición de default

`default = 1` si el crédito alcanza ≥ 90 días de mora en sus primeros 4 cortes. Se usan solo créditos con al menos 4 cortes observados: **2.938 créditos, 238 defaults, tasa de 8,1 %**.

## Modelo

Regresión logística sobre variables transformadas en WoE. Variables: recargas mensuales, pago a tiempo de telco, historial en buró, tipo de producto, monto solicitado e interacción con la app. No usa el sexo del cliente.

## Resultados (validación fuera de tiempo, oct–dic 2024)

| Métrica | Valor |
|---|---|
| AUC | 0,772 (IC95 0,72–0,83) |
| KS | 0,435 |
| PSI del score | 0,033 |

## Puntos de atención

* Los datos de telco y servicios públicos cambian de forma abrupta desde octubre de 2024; servicios públicos se excluyó del modelo.
* El modelo subestima ligeramente el riesgo reciente y debe recalibrarse.
* Recomendación: piloto controlado con montos bajos y monitoreo mensual del PSI.
