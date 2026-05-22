# 📊 Rúbrica Pedagógica: Pizzería Campus Express (10.0 pts)

| Criterio | Nivel 4 (Completo) | Nivel 3 (Parcial) | Nivel 2 (Mínimo) | Nivel 1 (No cumple) | Ptos |
|----------|-------------------|-------------------|------------------|---------------------|------|
| **1. Lógica FIFO/LIFO** | Revierte ambos casos (`PEDIDO`/`ENTREGADO`) sin errores de orden. | Revierte un caso correctamente, el otro presenta inconsistencia leve. | Solo funciona uno o el orden se pierde parcialmente. | No hay reversión o lógica invertida. | 2.5 |
| **2. Formato de Texto** | Uso correcto y consistente de `string.Format()` en todos los mensajes y contadores. | 1-2 mensajes sin formatear o concatenación manual excesiva. | Uso inconsistente de `string.Format()` o errores de placeholders `{0}`. | No se usa formateo estructurado. | 1.5 |
| **3. Interfaz WinForms** | UI refleja estado real tras cada clic. ListBox limpio y ordenado. | Actualización correcta, pero sin limpieza previa o diseño desordenado. | UI se desincroniza ocasionalmente o requiere reinicio manual. | No se actualiza o bloquea hilo principal. | 2.0 |
| **4. Validaciones y Seguridad** | Todas las operaciones validadas. Mensajes claros en `lblEstado`. | Falta 1 validación menor, pero no crashea. | >2 validaciones ausentes o manejo crudo de excepciones. | Excepciones no controladas en ejecución normal. | 1.5 |
| **5. Flujo Git + PR** | PR limpio, historial coherente, respuestas claras y contextualizadas. | Faltan 1 commit o respuestas incompletas, pero PR válido. | Solo 1 commit, mensajes genéricos, o respuestas ausentes. | No usa Fork/PR o sube ZIP/carpeta `.git`. | 1.5 |
| **6. Preguntas de Comprensión** | Respuestas precisas, usan terminología correcta y ejemplifican. | Respuestas correctas pero vagas o sin ejemplo concreto. | Solo 2-3 respuestas o contienen errores conceptuales leves. | Respuestas ausentes o completamente erróneas. | 1.0 |
| **TOTAL** | | | | | **10.0** |