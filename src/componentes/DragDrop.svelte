<script>
import interact from 'interactjs'
// import './DragDrop.css'
//
$: width = window.innerWidth
$: height = window.innerHeight
const resize = () => {
  window.onresize = (e) => {
    width = e.target.innerWidth
    height = e.target.innerHeight
  }
}
const itemDrag = '.dragall'
// colores paths
const colorDrag = 'orange'
const colorDragging = 'lime'
const colorArea = 'purple'
const colorEnterArea = 'lightgray'
const colorSnapped = 'gray'
// PATHS:
const pathsDrop = [
  {
    id: "circulo-area",
    class: "drop-1 dropall",
    d: "M532.65 254.94C532.65 352.36 453.56 431.45 356.14 431.45C258.73 431.45 179.64 352.36 179.64 254.94C179.64 157.52 258.73 78.43 356.14 78.43C453.56 78.43 532.65 157.52 532.65 254.94Z",
    opacity:"1",
    fill: colorArea
  },
  {
    id: "triangulo-area",
    class: "drop-2 dropall",
    d: "M426.63 374.82L180.84 377.23L300.12 86.87L426.63 374.82Z",
    opacity:"1",
    fill: colorArea
  },
  {
    id: "irregular-area",
    class: "drop-3 dropall",
    d: "M473.61 66.39L437.47 172.41L433.86 219.4L324.22 239.88L403.73 272.41L349.52 285.66L339.88 342.29L282.05 304.94L274.82 355.54L236.27 300.12L207.35 219.4L232.65 133.86L473.61 66.39Z",
    opacity:"1",
    fill: colorArea
  },
  {
    id: "cuadrado-area",
    class: "drop-4 dropall",
    d: "M112.17 79.64L495.3 79.64L495.3 459.16L112.17 459.16L112.17 79.64Z",
    opacity:"1",
    fill: colorArea
  }
]
const pathsDrag = [
  {
    id: "circulo-drag",
    class: "dragall",
    d: "M532.65 254.94C532.65 352.36 453.56 431.45 356.14 431.45C258.73 431.45 179.64 352.36 179.64 254.94C179.64 157.52 258.73 78.43 356.14 78.43C453.56 78.43 532.65 157.52 532.65 254.94Z",
    opacity:"1",
    fill: colorDrag
  },
  {
    id: "triangulo-drag",
    class: "dragall",
    d: "M426.63 374.82L180.84 377.23L300.12 86.87L426.63 374.82Z",
    opacity:"1",
    fill: colorDrag
  },
  {
    id: "irregular-drag",
    class: "dragall",
    d: "M473.61 66.39L437.47 172.41L433.86 219.4L324.22 239.88L403.73 272.41L349.52 285.66L339.88 342.29L282.05 304.94L274.82 355.54L236.27 300.12L207.35 219.4L232.65 133.86L473.61 66.39Z",
    opacity:"1",
    fill: colorDrag
  },
  {
    id: "cuadrado-drag",
    class: "dragall",
    d: "M112.17 79.64L495.3 79.64L495.3 459.16L112.17 459.16L112.17 79.64Z",
    opacity:"1",
    fill: colorDrag
  }
]
//
const Drag = () => {
  interact(itemDrag).draggable({
    inertia: true,
    modifiers: [
      interact.modifiers.restrictRect({
        restriction: 'svg',
        endOnly: true
      })
    ],
    // autoScroll: true,
    onmove: dragMoveListener,
    onend: (e) => {
      e.target.style.fill = colorDrag// area
      console.log('Fin')
    }

  })
}

const dragMoveListener = (e) => {

  let target = e.target
  console.log("Moviendo: ")
  // aplica los valores en los atributos data-
  var x = (parseFloat(target.getAttribute('data-x')) || 0) + e.dx
  var y = (parseFloat(target.getAttribute('data-y')) || 0) + e.dy
  // translate the element
  target.style.webkitTransform =
  target.style.transform =
  'translate(' + x + 'px, ' + y + 'px)'

  // update the position attributes
  target.setAttribute('data-x', x)
  target.setAttribute('data-y', y)
}
//
const Drop = (interactObj, acceptObj) => {

  interact(interactObj).dropzone({
    accept: acceptObj,
    overlap: 0.10,//10% encima del area
    // Eventos relacionados al 'drop':
    ondropactivate: (e) => {
      e.target.style.fill = colorDragging// area
      e.relatedTarget.style.fill = colorDragging//dragged obj
    },
    // Al entrar en zona de 'drop'
    ondragenter: (e) => {
      e.target.style.fill = colorEnterArea
      e.relatedTarget.style.fill = colorEnterArea
    },
    // Al soltarlo dentro de la zona de 'drop'
    ondrop: (e) => {
      let dropzone = e.target.getBoundingClientRect()
      let obj = e.relatedTarget
      let x = dropzone.x*0.001
      let y = dropzone.y*0.001
      // snap
      obj.style.webkitTransform =
      obj.style.transform =
      'translate(' + x + 'px, ' + y    + 'px)'
      obj.setAttribute('data-x', x)
      obj.setAttribute('data-y', y)
      //
      e.target.style.fill = colorSnapped
      e.relatedTarget.style.fill = colorSnapped
    },
    //Al salir del 'dropzone'
    ondragleave: (e) => {
      e.target.style.fill = colorDragging
      e.relatedTarget.style.fill = colorDragging
    },
    // Al dejar fuera de la zona de 'drop'
    ondropdeactivate: (e) => {
      e.target.style.fill = colorArea// area
    }
  })
}

function areaPos(e) {
  console.log(e);
}
//llamadas
resize()
Drag()
Drop("#cuadrado-area",'#cuadrado-drag')
Drop("#triangulo-area",'#triangulo-drag')
Drop("#irregular-area",'#irregular-drag')
Drop("#circulo-area",'#circulo-drag')
//
</script>
<!-- Estilos -->
<style>
* {
  /* outline: 1px solid black; */
}
#svg-contenedor {
  height: 100vh;
  width: 100vw;
}
</style>
<!-- HTML  -->
<section>
<!--  -->
<svg id="svg-contenedor" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
viewBox={`0 0 ${width} ${height}`}
width={`${width}px`}
height={`${height}px`}
>
<!-- Drops Areas -->
{#each pathsDrop as path}

<!-- on:create={areaPos()} -->
<path
on:create={event => created = event.target}
d={path.d}
id={path.id}
class={path.class}
opacity={path.opacity}
fill={path.fill}
></path>

{/each}

<!-- draggables -->
{#each pathsDrag as path}

<path
d={path.d}
id={path.id}
class={path.class}
opacity={path.opacity}
fill={path.fill}
></path>

{/each}

</svg>

</section>
