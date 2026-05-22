# 📘 GUÍA ESTUDIANTIL: Pizzería Campus Express
## Fundamentos, Lógica Práctica y Flujo de Implementación

> 📌 **Propósito:** Esta guía te acompaña durante la práctica de laboratorio. No contiene soluciones completas, pero sí el razonamiento, los pasos lógicos y las restricciones técnicas necesarias para que implementes el sistema con éxito.

---

## 🧠 1. Conceptos que Debes Dominar

### 🔹 FIFO vs LIFO en Contexto Real
| Estructura | Comportamiento | Analogía en la Pizzería | Clase en C# |
|------------|---------------|------------------------|-------------|
| **FIFO** | El primero en entrar es el primero en salir | Fila de pedidos: se atienden por orden de llegada | `Queue` / `Queue<T>` |
| **LIFO** | El último en entrar es el primero en salir | Bitácora de acciones: el último clic es el primero que se revierte | `Stack` / `Stack<T>` |

### 🔹 ¿Por qué usar ambas en el mismo sistema?
- La **cola** garantiza justicia operativa: nadie salta la fila.
- La **pila** garantiza control humano: los errores se corrigen revirtiendo la **última** acción, sin alterar el resto del flujo.
- Si las invirtieras, atenderías al último cliente primero y deshacerías el primer registro del día → caos operativo.

---

## ⚙️ 2. Mapeo Técnico a C# (Compatible SharpDevelop Legacy)

| Acción | Método | Clase |
|--------|--------|-------|
| Agregar a la cola | `.Enqueue(item)` | `Queue` o `Queue<string>` |
| Sacar de la cola | `.Dequeue()` | `Queue` o `Queue<string>` |
| Registrar acción | `.Push(texto)` | `Stack` o `Stack<string>` |
| Revertir acción | `.Pop()` | `Stack` o `Stack<string>` |
| Contar elementos | `.Count` | Ambas |
| Formatear texto | `string.Format("...", arg1)` | `System` |

> 💡 En SharpDevelop antiguos, si no reconoce `<string>`, usa `System.Collections` y convierte con `(string)cola.Dequeue()`.

---

## 🛠️ 3. Guía de Implementación (Razonamiento Paso a Paso)

> 📌 Abre `MainForm.cs` y sigue los `TODO`. Aquí tienes la lógica que debes aplicar en cada bloque.

### ✅ Paso 1: Registrar Nuevo Pedido (Entrada FIFO)
1. **Validación:** Lee `txtCliente.Text`, aplica `.Trim()`. Si está vacío, usa `string.Format()` para mostrar un mensaje en `lblEstado` y detén el flujo con `return;`.
2. **Cola:** Agrega el nombre con `colaPedidos.Enqueue(...)`. Esto respeta el orden de llegada.
3. **Bitácora:** Registra la acción con `pilaBitacora.Push(string.Format("PEDIDO: {0}", cliente));`. El prefijo es clave para el paso 3.
4. **UI:** Limpia el `TextBox`, actualiza `lblEstado` con confirmación y llama a `ActualizarUI()`.

### ✅ Paso 2: Preparar/Entregar (Salida FIFO)
1. **Validación:** Si `colaPedidos.Count == 0`, no hay qué atender. Muestra advertencia y retorna.
2. **Cola:** Extrae con `string entregado = (string)colaPedidos.Dequeue();` (usa cast si no usas genéricos).
3. **Bitácora:** Registra con `pilaBitacora.Push(string.Format("ENTREGADO: {0}", entregado));`.
4. **UI:** Actualiza estado e interfaz.

### ✅ Paso 3: Deshacer Última Acción (Lógica LIFO + Reversión)
> 💡 **Este es el núcleo del ejercicio.** No copies código; entiende la lógica.
1. **Validación:** Si `pilaBitacora.Count == 0`, no hay historial. Informa y retorna.
2. **Extracción:** `string ultimaAccion = (string)pilaBitacora.Pop();`
3. **Análisis:** Usa `.StartsWith("PEDIDO:")` o `.StartsWith("ENTREGADO:")` para saber qué revertir.
   - **Caso PEDIDO:** El cliente nunca salió de la cola. Debes **reconstruir la cola sin ese registro**. 
     - *Pista lógica:* Convierte la cola a arreglo temporal → limpia la cola original → itera el arreglo y vuelve a `Enqueue()` solo los elementos que **no coincidan** con el nombre extraído.
   - **Caso ENTREGADO:** El cliente ya fue despachado. Debes **volver a encolarlo**.
     - *Pista lógica:* Extrae el nombre usando `.Replace("ENTREGADO: ", "")` y haz `colaPedidos.Enqueue(nombre)`.
4. **UI:** Muestra en `lblEstado` qué se revirtió (usa `string.Format`) y sincroniza la interfaz.

### ✅ Paso 4: Sincronizar Interfaz
1. Limpia ambos `ListBox` con `.Items.Clear()`.
2. Itera `colaPedidos` con `foreach` y agrega cada elemento a `lstPedidos.Items`.
3. Itera `pilaBitacora` con `foreach` y agrega cada elemento a `lstBitacora.Items`.
4. Actualiza `lblContador` con `string.Format("Pedidos: {0} | Bitácora: {1}", colaPedidos.Count, pilaBitacora.Count)`.
5. Si `colaPedidos.Count == 0`, agrega un placeholder visual: `lstPedidos.Items.Add("(Sin pedidos pendientes)");`.

---

## 🌐 4. Flujo Git de Entrega (Profesional)
| Paso | Acción |
|------|--------|
| 1 | Ve al repositorio base → Presiona **Fork** → Crea la copia en tu cuenta. |
| 2 | Clona o descarga ZIP → Abre `.sln` en SharpDevelop. |
| 3 | Implementa siguiendo los `TODO` y esta guía. |
| 4 | Realiza **mínimo 3 commits** con mensajes descriptivos:  `feat`: interfaz inicial, `fix`: validaciones y lógica FIFO, `feat`: deshacer LIFO y string.Format |
| 5 | `git push` → Ve a tu fork → `Contribute` → `Open Pull Request` al repositorio base. |
| 6 | En la descripción del PR, responde las **4 Preguntas de Comprensión** y adjunta una captura. |

---

## ❓ 5. Preguntas de Comprensión (Para reflexionar y responder en el PR)
1. ¿Por qué un sistema de delivery usa `Queue` para los pedidos pero `Stack` para la bitácora? ¿Qué problema operativo surgiría si invertimos las estructuras?
2. ¿Por qué es obligatorio verificar `Count == 0` antes de `Dequeue()` o `Pop()`? ¿Qué ocurre en ejecución si se omite esta validación?
3. En el método `Deshacer`, ¿por qué es necesario analizar el texto con `.StartsWith()` antes de revertir? ¿Qué error de estado evitaría esto?
4. ¿Qué ventaja tiene entregar mediante Fork + Pull Request en lugar de un archivo comprimido? ¿Cómo facilita la evaluación, la trazabilidad y la retroalimentación?

---

## 💡 6. Consejos para el Laboratorio (SharpDevelop Legacy)
| Situación | Solución Rápida |
|-----------|-----------------|
| `ListBox` acumula datos | Olvidaste `.Items.Clear()` antes del `foreach`. |
| Error en `<string>` | Usa `System.Collections` y cast: `(string)cola.Dequeue()` |
| `Dequeue()` lanza excepción | Nunca llames sin validar `Count > 0` primero. |
| Git rechaza `push` | Ejecuta `git status`. Verifica que no estés subiendo `bin/` o `obj/`. |
| Depuración | `F9` (breakpoint) → `F5` (ejecutar) → `F10` (siguiente línea) |
| Atajos útiles | `Ctrl+Shift+B` (compilar), `Ctrl+S` (guardar todo) |

---

## ✅ 7. Checklist de Autoevaluación (Antes de hacer el PR)
- [ ] El proyecto abre y compila en SharpDevelop sin errores críticos.
- [ ] Uso consistente de `string.Format()` en todos los mensajes.
- [ ] `btnDeshacer_Click` revierte correctamente tanto `"PEDIDO:"` como `"ENTREGADO:"`.
- [ ] Todos los `ListBox` se limpian antes de recargar.
- [ ] Hay ≥3 commits con mensajes descriptivos.
- [ ] El PR incluye respuestas a las 4 preguntas + captura de pantalla funcional.
- [ ] Carpetas `bin/`, `obj/`, archivos `*.suo` y `*.user` están excluidos por `.gitignore`.

📥 **Entrega final:** 
1 Ve al repositorio base → Presiona Fork → Crea la copia en tu cuenta.
2 Clona o descarga ZIP → Abre .sln en SharpDevelop.
3 Implementa siguiendo los TODO y esta guía.
4 Realiza mínimo 3 commits con mensajes descriptivos: feat: interfaz inicial, fix: validaciones y lógica FIFO, feat: deshacer LIFO y string.Format
5 git push → Ve a tu fork → Contribute → Open Pull Request al repositorio base.
6 En la descripción del PR, responde las 4 Preguntas de Comprensión y adjunta una captura del formulario funcionando.