<script>
    export let data;
    export let xScale;
    export let yScale;
    export let width;

    $: console.log(tooltipWidth + x > width);

    let tooltipWidth;
    // usamos las escalas que ya hemos creado y usado para los círculos: así el tooltip aparecerá en la misma posición (es un poco como replicar lo de los circulos pero para que aparezca en X ocasiones y no un círculo sino un tooltip)
    $: x = xScale(data.porcentaje);
    $: y = yScale(data.year);

    $: xPosition =
        tooltipWidth + x > width ? x - tooltipWidth - xNudge : x + xNudge;
    $: yPosition = y + yNudge;

    const xNudge = 15;
    const yNudge = 30;

    import { fade } from "svelte/transition";
</script>

<!-- Creamos un div para que englobe el tooltip y aparezca en el punto donde ponemos el ratón y no al final de la página-->
<div
    transition:fade
    bind:clientWidth={tooltipWidth}
    class="tooltip"
    style="left: {xPosition}px; top: {yPosition}px;"
>
    <!-- Aquí irá el contenido del tooltip (se incluye como un HTML normal)-->
<!-- Muestra el país y las hectáreas quemadas en verano -->
    <h1>{data.pais} <span>({data.hec.toLocaleString()} ha quemadas ese año)</span></h1>    
    <p>El <span>{data.porcentaje}%</span> de las hectáreas quemadas en <span>{data.year}</span> 
        <br>
        fueron en incendios que se produjeron en verano</p>
</div>

<style>
    .tooltip {
        position: absolute;
        padding: 8px 6px;
        background: white;
        box-shadow: rgba(0, 0, 0, 0.15) 2px 3px 8px;
        border-radius: 3px;
        pointer-events: none; /* Esto es para que no se puedan hacer clic en el tooltip, solo en los círculos*/
        transition:
            top 300ms ease,
            left 300ms ease;
    }

    h1 {
        font-size: 1rem;
        font-weight: 500;
        margin-bottom: 6px;
        width: 100%;
    }

    p {
        font-size: 0.9rem;
        font-weight: 300;
    }

    p span {
        font-size: 100%;
        padding: 2px 4px;
        background-color: hsla(0, 100%, 38%, 0.502);
        margin-left: 2px;
        display: inline-block;
        border-radius: 3px;
    }

    h1 span {
        font-weight: 650;
        color: rgba(192, 0, 0, 1);
        font-size: 80%;
    }

</style>
