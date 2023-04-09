<script>
    export let data;
    export let category;
    export let width;
    export let xScale;
    export let yScale;
    export let color;
    export let idClip;

   
</script>

{#each data as d,i}
    <rect
        class={category}
        x={xScale(new Date(d.date))}
        y={yScale(d.y1)}
        width={  width / data.length}
        height={yScale(d.y0) - yScale(d.y1)}
        fill={color}
        clip-path={`url(#${idClip})`}
    />
    
{/each}