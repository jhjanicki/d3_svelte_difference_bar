<script>
    import * as d3 from "d3";
    export let height;
    export let xScale;

    //to format the x-axis labels
    const dateFormat = d3.timeFormat("%Y-%m");

    const TICK_LENGTH = 8;
    const ticks = xScale.ticks(20);
</script>

{#each ticks as tick}
    <line
        x1={xScale(tick)}
        x2={xScale(tick)}
        y1={height}
        y2={height + TICK_LENGTH}
        stroke="black"
    />
    <text
        x={xScale(tick)}
        y={height}
        dy="10"
        dominant-baseline="hanging"
        text-anchor="middle"
        fill="black"
    >
        {dateFormat(tick)}
    </text>
{/each}

<style>
    text {
        text-transform: uppercase;
        font-size: 14px;
    }
</style>