<script>
  // Import the data here
 
   import data from "$data/datos.js";

   //import data from "$data/data.js"; // llamamos data a lo que importamos y el símbolo "$" significa que coja el archivo sin importar dónde se encuentre en la carpeta (en este caso está en una sub carpeta)
   import { scaleLinear } from "d3-scale"; // importamos la librería d3-scale para usar Scalelinear


  const margin = {
    top: 20,
    right: 20,
    bottom: 20,
    left: 0,
  };

  let width = 500;
  $: innerWidth = width - margin.left - margin.right;
  let height = 500;
  let innerHeight = height - margin.top - margin.bottom;

  // creamos una escala para el eje x
  $: xScale = scaleLinear()
    .domain([0, 100]) // rango de los datos que metems
    .range([50, innerWidth]); // rango en el que queremos que salgan los datos

  // lo mismo para la escala de eje y
  let yScale = scaleLinear()
    .domain([2008, 2023])
    .range([innerHeight, 20]); // el eje y va de arriba a abajo

  // creamos una escala para el radio
  let rScale = scaleLinear()
    .domain([0, 390000]) // rango de hectáreas en los datos
    .range([4, 30]); // rango del radio de los círculos
  

  import AxisX from "$components/AxisX.svelte"; //este es el nombre del archivo y la ruta del archivo. Ahí crearemos el Eje
  //pero necesitamos USARLO dentro de nuestro gráfico, no basta solo con "importarlo" aquí
  import AxisY from "$components/AxisY.svelte";

  // creamos la variable hoveredData que vamos a usar para mostrar la información
  let hoveredData;
  // con el símbolo $ nos aseguramos de que hoveredData se actualice cada vez que se pase por encima de un círculo
  $: console.log(hoveredData);

  import Tooltip from "$components/Tooltip.svelte";
  import {fly} from "svelte/transition"

</script>

<h1>Evolución de los incendios veraniegos en Europa (2008-2023)</h1>
<h2>Porcentaje de hectáreas quemadas durante incendios de verano en varios países de Europa entre 2008 y 2023. <br> El tamaño de los círculos indica el total de hectáreas quemadas ese año </h2>
<div
  class="chart-container"
  bind:clientWidth={width}
  on:mouseleave={() => {
    hoveredData = null;
  }}
>
  <!-- Para que muestre los círculos necesitamos que estén contenidos en un svg y le asignamos la altura y anchura de los ejes-->
  <svg {width} {height}>
    <g transform="translate({margin.left}, {margin.top})">
      <!-- hemos creado unos margenes y transformamos el gráfico entero en base a ella creando un grupo -->
      <!--realmente esto equivale a width={width} height={height}, pero como la variable y el valor se llaman igual se puede acortar-->
      <AxisX {xScale} height={innerHeight} width={innerWidth} />
      <!-- para usar el eje X que hemos creado en el archivo-->
      <!-- xScale={xScale} o {xScale} para importar la función de escala que hemos usado para ubicar los círculos en el nuevo documento para los ejes-->
      <AxisY {yScale} width={innerWidth} />
      <!-- Svelte {#each} block -->
      <!-- creamos un círculo para cada dato-->
      {#each data as d}
        <!-- svelte-ignore a11y-mouse-events-have-key-events -->
        <circle
          in:fly={{
           /* con fly le estamos diciendo que empiece en x - 10 y opacidad 0 hasta alcanzar los valores normales que son los que asignamos al círculo */
            x: -10, opacity: 0, duration: 1000 }}
          cx={xScale(d.porcentaje)}
          cy={yScale(d.year)}
          r= {hoveredData === d ? rScale(d.hec) * 1.3 : rScale(d.hec)} 
          opacity={hoveredData // un if dentro de otro if: si hoveredData existe (o sea si hay algún círculo seleccionado) haz
            ? hoveredData === d
              ? 1
              : 0.4 // que solo el círculo seleccionado tenga un 1 de opacidad y el resto un 0.5
            : // sino hay ninguno seleccionado, todos con opacidad de 1 // {hoveredData === d ? 20 : 10}
              0.6}
          fill=#c00000
          stroke="black"
          stroke-width="1"
          on:mouseover={() => {
            hoveredData = d; // cambiamos el valor de hoveredData para que sea d
          }}
        />
      {/each}
    </g>
  </svg>
  {#if hoveredData}
    <Tooltip data={hoveredData} {xScale} {yScale} width={innerWidth} />
  {/if}
</div>

<style>
  /* así estamos seleccionando la clase "tick" que hemos añadido a los grupos de los ejes 
  y con text dentro de la clase .tick el elemento text
  PERO solo se aplica en este archivo. Así que lo envolvemos en :global() */
  :global(.tick text) {
    font-family: "PT Sans", sans-serif;
    font-weight: 400;
    font-size: 12px;
    fill: #000000;
    opacity: 80%;
  }

  :global(.axis-label) {
    font-family: "PT Sans", sans-serif;
    font-weight: 710;
    font-size: 13px;
    fill: #000000;
    opacity: 80%;
  }

  circle {
    transition:
      r 300ms ease,
      opacity 500ms ease;
  }

  h1 {
    font-family: "PT Sans", sans-serif;
    font-size: 1.7rem;
    font-weight: bold;
    margin-bottom: 0.4rem;
  }

  h2 {
    font-family: "PT Sans", sans-serif;
    font-size: 0.9rem;
    margin-bottom: 0.5rem;
  }

</style>
