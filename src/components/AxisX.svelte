<!-- La estructura de este archivo será similar a la de App.svelte -->
<script>
    export let xScale; // aunque parezca exportar, realmente estamos importando la función que hemos "llamado" en App.svelte para poder usarla
    export let height;
    export let width;

    let xTicks = xScale.ticks(4); // creamos la variable de los ticks del eje
</script>

<g class="xaxis" transform="translate(0, {height})">
    {#each xTicks as tick, index}
        <g class="tick" transform="translate({xScale(tick)}, 0)">
            <!-- creamos un grupo para que cada texto vaya con una linea de tick -->
            <line x1="0" x2="0" y1="0" y2="6" stroke="hsla(212, 10%, 53%, 1)"
            ></line>
            <text
                x={0} y={0} dy={17} dominant-baseline= "middle" text-anchor="middle">
                <!-- con esta variable damos un valor 0 a la x porque lo  va a modificar el grupo poniendo el valor de la altura -->
                <!-- Con text-anchor estamos diciendo que el valor con indice 0 (que en este caso coincide con el número 0), lo queremos en el medio en vez de a la derecha porque sino no aparece en el gráfico. El resto se quedan normales-->
                {tick}% <!-- El valor que hay dentro de tick (que en este caso es igual al valor de la posición X del tick) pero va fuera de los atributos del texto-->
            </text>
        </g>
    {/each}
    <text x={width} 
    y=-460
    dominant-baseline="start" 
    text-anchor="end"
    class="axis-label">
        Porcentaje 🡢
    </text>
</g>

<!-- dy property -->
<!-- dominant-baseline property -->
<!-- text-anchor property-->