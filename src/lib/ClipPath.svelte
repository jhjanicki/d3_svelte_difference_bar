<script>
    import * as d3 from "d3";
    export let data;
    export let xScale;
    export let yScale;
    export let y0;
    export let idClip;
    

    const areaGenerator = d3.area()
        .x(d=> xScale(new Date(d.date.toString())))
        .y0(yScale(y0))
        .y1(d=> yScale(d.mean))
        .curve(d3.curveStepAfter)
    
</script>

<clipPath id={idClip}>
    <path 
        d={areaGenerator(data)}
    />
</clipPath>
