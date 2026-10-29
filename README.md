# Entrega · Laboratorio 3.1 · Introducción a la automatización de pruebas

**Equipo:** `equipo-N` · **Integrantes:** _nombres_ · **Repositorio:** _enlace a este repositorio_

> Respondan aquí, en este archivo, y háganle commit. Sean breves y concretos: una frase buena vale más que un párrafo.

## 1. ¿Qué automatizamos y por qué? (decisión)

| Criterio (US-xx · CAy) | ¿Automatizamos? | Por qué (frecuencia · riesgo · estabilidad · facilidad de verificar) |
|---|---|---|
| US-04 · CA1 (agregar 15 cuadernos, stock 15) | Sí | Se repite en cada compra, es el caso límite del stock y el resultado es un número fácil de comprobar. |
| US-04 · CA3 (4 + 4 mochilas supera el stock 6) | Sí | Riesgo alto (vender lo que no hay) y cazabugs: una persona no repetiría la secuencia con la misma exactitud cada vez. |
| US-06 · CA2 (con cantidad 1, "−" elimina el producto) | Sí | Regla estable, y de ella depende que el total nunca llegue a cero con productos en el carrito. |
| US-07 · CA2 (quitar el único producto → carrito vacío y total $ 0) | Sí | Flujo repetitivo, verificable con dos aserciones (línea ausente y Subtotal $ 0). |
| US-09 · CA3 (DESC10, desc10 y Desc10 se reconocen igual) | Sí | Tres variantes de la misma regla: automatizar una tabla de datos cuesta casi nada y a mano es tedioso. |
| US-10 · CA2 (código inválido no borra un descuento válido) | Sí | Riesgo económico alto y el total es fácil de comparar antes y después. |
| US-11 · CA2 (IVA sobre el valor descontado) | Sí | Dinero, regla estable y la máquina calcula el valor esperado mejor y más rápido que una persona. |
| US-12 · CA2 (Total = subtotal − descuento + IVA) | Sí | Mismo riesgo que US-11, es la cifra que el cliente paga. |
| US-13 · CA3 (con total cero no se puede ir a pagar) | Sí | Riesgo alto (cobrar $ 0 o valores negativos) y se verifica con un solo `toBeDisabled()`. |
| US-14 · CA1 (formulario vacío → un error por campo) | Sí | Validación estable, se ejecuta en cada cambio del checkout y el texto del mensaje es exacto. |
| US-15 · CA1–CA4 (correos válidos y mal formados) | Sí | Muchas entradas con el mismo flujo: ideal para pruebas dirigidas por datos (12 casos con un solo cuerpo). |
| US-17 · CA3 (el número de pedido no se repite tras recargar) | Sí | Riesgo alto (pedidos duplicados) y a mano es lento: requiere comprar, recargar y volver a comprar. |
| US-16 · CA1 (el formulario aparece vacío en la compra siguiente) | Sí | Privacidad de datos personales: riesgo alto y fácil de verificar con `toHaveValue("")`. |

## 2. Bugs que nuestra suite detecta

| Prueba (archivo › título) | Bug en Jira (clave) | ¿Quedó con `test.fail(...)`? | Evidencia (captura o trace) |
|---|---|---|---|
| 02-carrito › US-04 CA3: 4 + 4 mochilas supera el stock (6) y debe rechazarse | LBH-__ | Sí | `test-results/…US-04…CA3…/error-context.md` · aviso vacío, la tienda agrega igual |
| 02-carrito › US-05 CA2: "+" no pasa del stock (6 mochilas) | LBH-__ | Sí | `test-results/…US-05…/error-context.md` |
| 02-carrito › US-06 CA2: con cantidad 1, "−" elimina el producto | LBH-47 | Sí | `test-results/…US-06…/error-context.md` |
| 03-descuentos-iva › US-09 CA3: desc10 y Desc10 se reconocen igual que DESC10 (2 pruebas, 1 bug) | LBH-__ | Sí | `test-results/…US-09…/error-context.md` |
| 03-descuentos-iva › US-10 CA2: código inválido NO borra un descuento válido | LBH-__ | Sí | `test-results/…US-10…/error-context.md` |
| 03-descuentos-iva › US-11 CA2: el IVA se calcula sobre el valor descontado | LBH-__ | Sí | `test-results/…US-11…/error-context.md` |
| 03-descuentos-iva › US-12 CA2: total = subtotal − descuento + IVA | LBH-__ (probablemente el mismo de US-11: el IVA se calcula sobre el subtotal sin descuento) | Sí | `test-results/…US-12…/error-context.md` |
| 04-checkout › US-13 CA3: con total cero no se puede ir a pagar | LBH-__ | Sí | `test-results/…US-13…/error-context.md` |
| 04-checkout › US-15 CA1-CA3: correos válidos rechazados (ana.gomez@, ana-gomez@, ana+tienda@, ana@universidad.edu.co) | LBH-__ (un bug de validación de correo, 4 pruebas) | Sí | `test-results/…US-15…/error-context.md` |
| 05-confirmacion › US-16 CA1: el formulario aparece vacío tras otra compra | LBH-__ | Sí | `test-results/…US-16…/error-context.md` |
| 05-confirmacion › US-17 CA3: un pedido tras recargar no repite número | LBH-__ | Sí | `test-results/…US-17…/error-context.md` |
| heredados › H2: el IVA de 2 mochilas con DESC20 | mismo bug de US-11 | Ver sección 3 | Salida real: IVA $ 24.700 (19 % de 130.000, sin descontar) |

## 3. Pruebas heredadas: nuestro diagnóstico

| Prueba | Veredicto (producto / prueba / inestable / entorno) | Cómo lo comprobamos | Qué hicimos |
|---|---|---|---|
| H1 · el catálogo tiene 6 productos | **Prueba** (defecto de la prueba) | La tienda muestra 8 productos y así lo exige US-01 CA1 (ejemplo resuelto); la prueba esperaba 6, un dato viejo. | Corregimos el valor esperado a 8 y el título a "8 productos". |
| H2 · el IVA de 2 mochilas con DESC20… | **Producto** (bug) | Cálculo a mano desde el requisito: 2 × 65.000 = 130.000; − 20 % = 104.000; IVA 19 % = **19.760**. La tienda muestra $ 24.700 = 19 % de 130.000: calcula el IVA sin descontar. Es el mismo bug de US-11/US-12. | Restaurar la aserción a `$ 19.760` y marcarla con `test.fail(true, 'BUG · IVA sin descuento')`. No se ajusta la prueba al valor erróneo de la tienda. |
| H3 · agregar un cuaderno justo después de cargar | **Inestable** (condición de carrera) | Usa `waitUntil: 'domcontentloaded'` y hace clic de inmediato: a veces la página aún no está lista para responder al clic. Se comprueba con `npx playwright test heredados -g H3 --repeat-each=10` (pasa unas veces y falla otras). | Quitar el `waitUntil: 'domcontentloaded'` (o esperar a que el botón esté habilitado) y repetir con `--repeat-each=10` hasta que dé 10/10. |

## 4. Medidas

- Casos de prueba de nuestro plan en Jira: **\_\_** · automatizados en este laboratorio: **25 criterios** (41 pruebas Playwright con datos y heredadas) (**\_\_ %**)
- Tiempo que nos tomó escribir las pruebas automatizadas: **\_\_ min** · tiempo de ejecutar toda la suite: **\_\_ s** (ver "Duration" en `npm run report`)
- Tiempo estimado de ejecutar **a mano** esos mismos casos: **\_\_ min**
- Con esos números, ¿en cuántas ejecuciones se paga la automatización? (inversión ÷ ahorro por ejecución): **\_\_** ( = min de escritura ÷ (min a mano − min de la suite) )

## 5. Reflexión (máximo 5 líneas cada una)

1. Dejaríamos manuales lo visual y de usabilidad (diseño, textos, qué tan claro es un aviso), la exploración libre y lo que se prueba una sola vez: automatizarlo cuesta más de lo que ahorra. Tampoco automatizaríamos nada cuyo comportamiento aún no esté definido.
2. Si cambia el HTML de un botón, se rompen las pruebas que escriben el localizador directamente (la mayoría, con `.rounded-lg` copiado en cada archivo); no se rompen las que usan `TiendaPage`, porque ahí se corrige un solo lugar. El Page Object separa "qué hace el usuario" de "cómo se encuentra en el HTML".
3. H1 y H3. En H1 el mensaje decía "esperado 6, recibido 8", y el backlog (US-01 CA1) pide 8: la prueba estaba desactualizada. H3 no falla siempre: con `--repeat-each=10` pasa unas veces y otras no, lo que apunta a una espera mal hecha y no al producto.
4. Lo visual (un botón tapado, un texto ilegible), la usabilidad, la accesibilidad real y lo que ningún criterio de aceptación describe. La automatización solo comprueba lo que le escribimos; no descubre lo que no pensamos preguntar.
5. Fijaríamos primero el Page Object y datos de prueba en un solo lugar, y evitaríamos que las pruebas "cazabugs" fallen por un error nuestro: con `test.fail` una prueba mal escrita (por ejemplo, un `click()` sin `await`) también "pasa", y nos engaña. Revisaríamos esas pruebas con la regla "falla por la razón correcta", y registraríamos cada bug en Jira antes de marcar `test.fail`.