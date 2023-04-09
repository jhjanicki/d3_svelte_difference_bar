<script>
    import * as d3 from "d3";
    export let data;
    export let xScale;
    export let yScale;
    

    const lineGenerator = d3.line()
        .x(d=> xScale(new Date(d.date.toString())))
        .y(d=> yScale(d.mean))
        .curve(d3.curveStepAfter)
    
</script>

    <path 
        d={lineGenerator(data)}
        fill="none" 
        stroke="#4d4d4d"
        stroke-width="1"
    />
