# Centro de Eventos

## El problema

Se pidió una app que demostrara de forma práctica cómo Vue maneja eventos del DOM: modificadores de eventos, modificadores de teclado y el control de la propagación (bubbling).

## La solución

Un "Centro de Eventos" con tres módulos interactivos donde cada acción del usuario queda registrada al instante en un panel de log, para ver en tiempo real el efecto de cada modificador.

## Cómo lo resolví

- **Modificadores de eventos**: `@click.once`, `@click.self` y `@click.prevent`/`@submit.prevent` aplicados a casos reales (un botón que deja de reaccionar, un contenedor que ignora clics en sus hijos, un link y un formulario que no ejecutan su comportamiento por defecto).
- **Modificadores de teclado**: un input con `@keyup.enter`, `.esc`, `.delete` y la combinación `.ctrl.enter`, cada uno logueado por separado.
- **Propagación de eventos**: tres capas anidadas con un checkbox que activa `event.stopPropagation()` en la capa interior, permitiendo comparar en el mismo registro el comportamiento con y sin bubbling.

## Features

- Registro de eventos en tiempo real con marca de hora.
- Demostración lado a lado de cada modificador de eventos y de teclado.
- Comparación interactiva de propagación con y sin `stopPropagation()`.

## Tecnologías

- [Vue 3](https://vuejs.org/) (Composition API)
- [Vite](https://vitejs.dev/)
- JavaScript
