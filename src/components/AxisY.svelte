<script>
    export let yScale;
    export let width;
    let yTicks = [2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020, 2021, 2022, 2023]; // utilizamos la funcion ticks de scale que te da ticks de forma automatica (no siempre exactamente el numero que pones, pero lo intenta en base a los datos que tienens)
</script>

<g>
    {#each yTicks as tick, index}
        <g class="tick" transform="translate(0, {yScale(tick)})">
            <line
                x1="0"
                y1="0"
                y2="0"
                x2={width}
                stroke={index === 0 ? "#8f8f8f" : "#e5e7eb"}
            ></line>
            <text x="0" y="-4"> {tick} </text>        </g>
    {/each}

<text x="26"
    y="0" 
    dy="-6" 
    dominant-baseline="end" 
    text-anchor="end"
    class="axis-label">
        Año
</text>

</g>
