<script>
import interact from 'interactjs'
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
const paths = [
  {
    id: "circulo-area",
    class: "drop-1 dropall",
    d: "M232.65 554.94C232.65 652.36 653.56 531.45 456.14 531.45C258.73 531.45 379.64 552.36 379.64 554.94C279.64 257.52 258.73 178.43 356.14 378.43C453.56 178.43 232.65 457.52 232.65 554.94Z",
    opacity:"1",
    fill: colorArea
  },
  {
    id: "triangulo-area",
    class: "drop-2 dropall",
    d: "M284.42 249.88L120.56 251.48L200.08 57.91L284.42 249.88Z",
    opacity:"1",
    fill: colorArea
  },
  {
    id: "irregular-area",
    class: "drop-3 dropall",
    d: "M236.805 33.195L218.735 86.205L216.93 109.7L162.11 119.94L201.865 136.205L174.76 142.83L169.94 171.145L141.025 152.47L137.41 177.77L118.135 150.06L103.675 109.7L116.325 66.93L236.805 33.195Z",
    opacity:"1",
    fill: colorArea
  },
  {
    id: "cuadrado-area",
    class: "drop-4 dropall",
    d: "M28.042 19.91L123.8 19.91L123.8 114.79L28.042 114.79L28.042 19.91Z",
    opacity:"1",
    fill: colorArea
  },
  {
    id: "circulo-drag",
    class: "dragall",
    d: "M232.65 554.94C232.65 652.36 653.56 531.45 456.14 531.45C258.73 531.45 379.64 552.36 379.64 554.94C279.64 257.52 258.73 178.43 356.14 378.43C453.56 178.43 232.65 457.52 232.65 554.94Z",
    opacity:"1",
    fill: colorDrag
  },
  {
    id: "triangulo-drag",
    class: "dragall",
    d: "M284.42 249.88L120.56 251.48L200.08 57.91L284.42 249.88Z",
    opacity:"1",
    fill: colorDrag
  },
  {
    id: "irregular-drag",
    class: "dragall",
    d: "M236.805 33.195L218.735 86.205L216.93 109.7L162.11 119.94L201.865 136.205L174.76 142.83L169.94 171.145L141.025 152.47L137.41 177.77L118.135 150.06L103.675 109.7L116.325 66.93L236.805 33.195Z",
    opacity:"1",
    fill: colorDrag
  },
  {
    id: "cuadrado-drag",
    class: "dragall",
    d: "M28.042 19.91L123.8 19.91L123.8 114.79L28.042 114.79L28.042 19.91Z",
    opacity:"1",
    fill: colorDrag
  }
]
//
const configuraDrag = () => {
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
const configuraDrop = (interactObj, acceptObj) => {

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
      let x = 0//dropzone.x
      let y = 0//dropzone.y
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

//llamadas
resize()
configuraDrag()
configuraDrop("#cuadrado-area",'#cuadrado-drag')
configuraDrop("#triangulo-area",'#triangulo-drag')
configuraDrop("#irregular-area",'#irregular-drag')
configuraDrop("#circulo-area",'#circulo-drag')
//
</script>
<!-- Estilos -->
<style>
#svg-contenedor, section {
  height: 100%;
  width: 100%;
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
  {#each paths as path}
    <path
    on:create={e => console.log(e.target)}
    d={path.d}
    id={path.id}
    class={path.class}
    opacity={path.opacity}
    fill={path.fill}
    ></path>
  {/each}

</svg>

</section>
