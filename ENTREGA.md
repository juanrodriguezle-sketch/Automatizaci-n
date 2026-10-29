# Entrega · Laboratorio 3.1 · Introducción a la automatización de pruebas

**Equipo:** `Estoy solo` · **Integrantes:** Juan José Rodriguez Gonzalez · **Repositorio:** [text](https://github.com/juanrodriguezle-sketch/Automatizaci-n)

> Respondan aquí, en este archivo, y háganle commit. Sean breves y concretos: una frase buena vale más que un párrafo.

## 1. ¿Qué automatizamos y por qué? (decisión)

Elijan **mínimo 10 criterios de aceptación** de los que automatizaron (o descartaron). Un buen candidato es repetitivo, estable, de riesgo alto y fácil de verificar por una máquina.

| Criterio (US-xx · CAy) | ¿Automatizamos? | Por qué (frecuencia · riesgo · estabilidad · facilidad de verificar) |
|---|---|---|
| _US-04 · CA2_ | Sí / No / Más adelante | _…_ |

RESPUESTA 

US-01 · CA1 — Ver los 8 productos del catálogo/	Sí	/Es una prueba repetitiva que se ejecuta después de cada cambio. Existe el riesgo de que no se carguen todos los productos y es fácil comprobar la cantidad automáticamente.

US-02 · CA1 — Actualizar el stock al agregar productos/	Sí	/Se ejecuta frecuentemente y evita errores en las unidades disponibles. Es estable y permite comparar automáticamente el stock inicial con el resultado esperado.

US-03 · CA2 — Validar cantidades vacías, cero o negativas/	Sí	/Es una validación repetitiva que evita cantidades incorrectas. Se pueden probar fácilmente diferentes valores de entrada y comprobar el resultado esperado.

US-04 · CA2 — Impedir agregar más unidades que el stock/	Sí	/Tiene un riesgo alto porque puede generar pedidos que no se pueden cumplir. Es fácil automatizar la prueba con un stock conocido y verificar que el sistema rechace el exceso.

US-05 · CA2 — Impedir aumentar la cantidad por encima del stock/	Sí	/Es una prueba frecuente del carrito. Evita inconsistencias en las existencias y permite comprobar automáticamente que la cantidad no cambie cuando alcanza el límite.

US-07 · CA2 — Mostrar el carrito vacío y total de $0/	Sí	/Es una función fácil de verificar y se puede repetir en cada ejecución. Reduce el riesgo de mostrar cobros incorrectos cuando se elimina el último producto.

US-09 · CA1 — Aplicar el descuento DESC10 del 10 %/	Sí	/Los cálculos se repiten en muchas compras y un error afecta el dinero que paga el cliente. Es estable y permite comparar el descuento calculado con el valor esperado

US-11 · CA1 — Calcular el IVA del 19 % sin descuento/	Sí	/Es una operación matemática repetitiva y de riesgo alto por su efecto en el precio final. Se puede verificar automáticamente comparando el IVA calculado con el 19 % del subtotal.

US-14 · CA3 — Rechazar nombres que solo contienen números o símbolos/	Sí	/Es una validación frecuente de los datos del cliente. Evita información incorrecta y resulta fácil probar diferentes entradas para verificar que el sistema las rechace.

US-15 · CA4 — Rechazar correos sin arroba, dominio o con espacios/	Sí	/Se puede ejecutar con muchos ejemplos de correos válidos e inválidos. Reduce errores en los datos de contacto y es fácil comparar los resultados con las reglas definidas.
## 2. Bugs que nuestra suite detecta

Una fila por cada prueba que falló por un bug **del producto**. Enlacen el bug con su clave de Jira y agreguen en Jira un comentario con el nombre de la prueba que lo detecta.

| Prueba (archivo › título) | Bug en Jira (clave) | ¿Quedó con `test.fail(...)`? | Evidencia (captura o trace) |
|---|---|---|---|
| _02-carrito › CA3: 4 + 4 mochilas…_ | _PRJ-12_ | Sí | _…_ |

## 3. Pruebas heredadas: nuestro diagnóstico

| Prueba | Veredicto (producto / prueba / inestable / entorno) | Cómo lo comprobamos | Qué hicimos |
|---|---|---|---|
| H1 · el catálogo tiene 6 productos | La prueba estaba con datos erroneos | Corriendo y analizando el códgo | Cambiar algunas cosas de código y volverlo a correr |
| H2 · el IVA de 2 mochilas con DESC20… | La prueba estaba con datos erroneos | Corriendo y analizando el códgo | Cambiar algunas cosas de código y volverlo a correr |
| H3 · agregar un cuaderno justo después de cargar | La prueba estaba con datos erroneos | Corriendo y analizando el códgo | Cambiar algunas cosas de código y volverlo a correr |

## 4. Medidas

- Casos de prueba de nuestro plan en Jira: **\_\_** · automatizados en este laboratorio: **\_\_** (**\_\_ %**)
- Tiempo que nos tomó escribir las pruebas automatizadas: **\_\_ min** · tiempo de ejecutar toda la suite: **\_\_ s**
- Tiempo estimado de ejecutar **a mano** esos mismos casos: **\_\_ min**
- Con esos números, ¿en cuántas ejecuciones se paga la automatización? (inversión ÷ ahorro por ejecución): **\_\_**


RESPUESTA 

MEDIDAS

Casos de prueba de nuestro plan en Jira: 7; automatizados en este laboratorio: 7 (100 %).
Tiempo que nos tomó escribir las pruebas automatizadas: 60 min; tiempo de ejecutar toda la suite: 15 s.
Tiempo estimado de ejecutar a mano esos mismos casos: 7 min.
¿EN cuántas ejecuciones se paga la automatización?: aproximadamente 9 ejecuciones.

## 5. Reflexión (máximo 5 líneas cada una)

1. ¿Qué casos de nuestro plan **no** automatizaríamos y por qué?
2. Si la tienda cambia el HTML de un botón, ¿qué parte de nuestro código se rompe y cuál no? ¿Qué aportó el Page Object?
3. Una prueba roja nuestra que **no** era un bug del producto: ¿cómo nos dimos cuenta?
4. ¿Qué bug o riesgo **no** habría encontrado la automatización por sí sola?
5. ¿Qué haríamos distinto en un proyecto real?


REFLEXION 

1. ¿Qué casos de nuestro plan no automatizaríamos y por qué?

No automatizaríamos inicialmente las pruebas visuales, la facilidad de uso y la apariencia de la tienda. Estas pruebas requieren observación humana y permiten identificar problemas de diseño que una prueba automática podría pasar por alto.

2. Si la tienda cambia el HTML de un botón, ¿qué parte de nuestro código se rompe y cuál no? ¿Qué aportó el Page Object?

Se puede romper el localizador del botón si cambia su HTML. La lógica de la prueba no debería cambiar si se utiliza un Page Object, porque los localizadores y las acciones están centralizados en una clase independiente.

3. Una prueba roja muestra que no era un bug del producto: ¿cómo nos dimos cuenta?

Revisamos el mensaje de error, los pasos ejecutados y el resultado esperado. Después comprobamos manualmente la funcionalidad y encontramos que el problema estaba en la prueba automatizada, no en el producto.

4. ¿Qué bug o riesgo no habría encontrado la automatización por sí sola?

Podría no detectar problemas de usabilidad, colores difíciles de distinguir, textos confusos o una experiencia de compra poco intuitiva. Para encontrarlos, necesitamos pruebas manuales y evaluación de usuarios.

5. ¿Qué haríamos distinto en un proyecto real?

Definiríamos los casos prioritarios según el riesgo, usaríamos datos de prueba controlados y ejecutaríamos las pruebas automáticamente con cada cambio del código. También integraríamos las pruebas en un proceso de integración continua y mantendríamos los casos actualizados.