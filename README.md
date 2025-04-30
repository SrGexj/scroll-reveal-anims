# Animaciones con IntersectionObserver

Este repositorio ofrece dos funciones JavaScript que facilitan la animación de elementos al entrar en el viewport (área visible) usando la API de `IntersectionObserver`:

- `animateSingleElements()`: Añade la clase `inView` a cada elemento que tenga el atributo `data-anim` cuando aparece en pantalla.
- `animateChildElements()`: Para cada contenedor con `data-anim="parent"`, añade secuencialmente la clase `inView` a sus hijos (`data-anim="child"`) con un pequeño retardo.

---

## 📥 Instalación

1. Clona este repositorio o copia los archivos en tu proyecto:
   ```bash
   git clone https://github.com/tu-usuario/tu-repo.git
   ```
2. Asegúrate de tener en el repositorio:
   - `animations.js` con las funciones JS.
   - `_animations.scss` con los estilos SCSS.

```html
<link rel="stylesheet" href="path/to/_animations.css">
<!-- _animations.scss compilado a CSS -->
<script src="path/to/animations.js"></script>
```

---

## ⚙️ Uso

### 1. Animar elementos individuales

1. Añade el atributo `data-anim` a cualquier elemento que quieras animar al entrar en vista:
   ```html
   <div data-anim>
     Contenido animado
   </div>
   ```
2. Llama a la función `animateSingleElements()` después de que el DOM esté cargado:
   ```html
   <script>
     document.addEventListener('DOMContentLoaded', () => {
       animateSingleElements();
     });
   </script>
   ```

> Cada vez que un elemento con `data-anim` cruce al menos un 20% de su área visible, se le añadirá la clase `inView`.

### 2. Animar múltiples hijos secuencialmente

1. Marca un contenedor con `data-anim="parent"`:
   ```html
   <ul data-anim="parent">
     <li>Item 1</li>
     <li>Item 2</li>
     <li>Item 3</li>
   </ul>
   ```
2. La función asignará automáticamente `data-anim="child"` a cada hijo y los animará uno tras otro.
3. Llama a `animateChildElements()`:
   ```html
   <script>
     document.addEventListener('DOMContentLoaded', () => {
       animateChildElements();
     });
   </script>
   ```

> Cada hijo esperará `150ms * índice` antes de recibir la clase `inView` y dejará de observarse tras animarse.

---

## 🛠️ Opciones y personalización

Ambas funciones usan internamente un `IntersectionObserver` con estas opciones por defecto:

| Opción       | Valor    | Descripción                             |
| ------------ | -------- | ----------------------------------------|
| `root`       | `null`   | Observa con respecto al viewport        |
| `rootMargin` | `0px`    | Sin margen adicional                    |
| `threshold`  | `0.2–0.3`| Porcentaje de visibilidad para disparar |

Puedes modificar estos valores directamente en el código si necesitas otro comportamiento.

> Para un retardo distinto en los hijos, cambia el valor `150` dentro de `setTimeout`.

---

## 🎨 Estilos SCSS

Define los estilos SCSS para las animaciones que desees. Ejemplo:

```scss
// estilos para animaciones

[data-anim] {
  opacity: 0;
  transition: all .3s ease-out;
}

[data-anim="child"] {
  opacity: 0;
  transform: translateY(50px);
}

[data-anim="show-in"] {
  opacity: 0;
  transform: translateY(50px);
}

[data-anim="show-out"] {
  opacity: 0;
  transform: translateY(-50px);
}

[data-anim="show-left"] {
  opacity: 0;
  transform: translateX(50px);
}

[data-anim="show-right"] {
  opacity: 0;
  transform: translateX(-50px);
}

[data-anim="fade-in"] {
  opacity: 0;
}

.inView {
  opacity: 1;
  transform: translateY(0);
}
```

> Puedes modificar los valores de `translate` o la duración de `transition` para ajustar el efecto a tu diseño.

---

## 📖 Ejemplo completo

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Demo Animaciones</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>

  <h1 data-anim>Bienvenido</h1>

  <section data-anim="parent">
    <div>Tarjeta 1</div>
    <div>Tarjeta 2</div>
    <div>Tarjeta 3</div>
  </section>

  <script src="animations.js"></script>
  <script>
    document.addEventListener('DOMContentLoaded', () => {
      animateSingleElements();
      animateChildElements();
    });
  </script>
</body>
</html>
```

---

## ❓ Preguntas frecuentes

- **¿Funciona en todos los navegadores?**
  Intersection Observer está soportado en la mayoría de navegadores modernos. Para navegadores antiguos, puedes incluir un polyfill: [W3C Intersection Observer Polyfill](https://github.com/w3c/IntersectionObserver/tree/main/polyfill).

- **¿Puedo reiniciar la animación cuando el elemento sale de la vista?**
  Sí, elimina `observer.unobserve(entry.target)` y, en el `else` de `entry.isIntersecting`, quita la clase `inView`.

---

## 📄 Licencia

Este proyecto está bajo la licencia MIT. ¡Siéntete libre de usarlo y adaptarlo!
