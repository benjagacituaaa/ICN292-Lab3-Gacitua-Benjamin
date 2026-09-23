# ICN292 - Laboratorio 3

**Nombre:** Benjamin Sebastian Gacitúa Paredes
**RUT (sin dígito verificador):** 21.431.497-5
**Semilla S:** 497
**Fecha:** 23-09-2026

## Parámetros aplicados

| Parámetro | Cálculo | Valor |
|---|---|---|
| Umbral de monto U | 30000 + 1000 × (497 mod 50) | 77000 |
| Plazo máximo D | 7 + 7 × (497 mod 4) | 14 |

## Archivos

| Archivo | Descripción |
|---|---|
| `ICN292-Lab3-Gacitua-Benjamin.pdf` | Informe |
| `ICN292-Lab3-Gacitua-Benjamin.docx` | Informe, fuente editable |
| `ICN292-Lab3-Gacitua-Benjamin-triage.json` | Workflow de triage (Parte A) |
| `ICN292-Lab3-Gacitua-Benjamin-emisor.json` | Workflow emisor de las 15 solicitudes |
| `ICN292-Lab3-Gacitua-Benjamin-resumen.json` | Workflow programado de resumen (Parte B) |
| `capturas/` | Capturas del panel de ejecución |

## Cómo reproducir

1. En n8n: menú del workflow (⋯) → **Import from File** → seleccionar el `.json`.
2. Importar en este orden: triage, resumen, errorhandler, emisor.
3. **Credenciales.** Los nodos de Google Sheets y Gmail referencian credenciales por su id interno; los tokens no se exportan. Al importar hay que conectar una cuenta propia de Google en esos nodos.
4. **Registro.** El nodo Registro escribe en una planilla con estas columnas en la fila 1:
   `timestamp, id_solicitud, sku, monto, dias_desde_compra, estado_producto, email_cliente, ruta, motivo, umbral_U, plazo_D, valor_uf, monto_uf`
5. **Triage.** Publicar el workflow y copiar la Production URL del nodo Webhook (path `triage-497`).
6. **Emisor.** Pegar esa URL en el nodo HTTP Request y ejecutar. El batching está en 1 solicitud cada 5000 ms.
7. **Resumen.** Ejecutar manualmente o esperar el Schedule Trigger.

## Notas

- API externa consumida: `https://mindicador.cl/api` (GET, sin autenticación). Campo utilizado: `uf.valor`.
- El nodo de notificación arma el mensaje al cliente pero no lo envía.
- El flujo de resumen (Parte B) sí envía por Gmail: el nodo Enviar Resumen despacha el consolidado diario.
- No se realizó la parte C debido al vencimiento del free trial de n8n.
- En las capturas pueden aparecer dos montos distintos de Monto Uf, debido a que este valor cambia día a día, y el laboratorio se avanzó en dias distintos. 
