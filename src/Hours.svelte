<script>
import { fly } from "svelte/transition"

export let data

let width = 120,
    height = 18,
    padding = 3,
    max = Math.max(...data),
    min = Math.min(...data),
    span = max - min,
    points = ""

// Build polyline points
data.forEach((h, i) => {
    points += 
        ((i * ((width - padding * 2) / 23)) + padding).toFixed(0)
        + "," + 
        ((height - padding * 2) - (h - min) / (span / (height - padding * 2)) + padding).toFixed(1)
        + " "
})

let hour = "", 
    value = ""
function highlight(i) {
    if (i === undefined) {
        hour = value = ""
    }
    else {
        hour = "Kl " + parseInt(i).toLocaleString('nb-NO', { minimumIntegerDigits: 2 }) + "-" + parseInt(i+1).toLocaleString('nb-NO', { minimumIntegerDigits: 2 }) + ": "
        value = "<strong>" + parseInt(data[i]) + "</strong> øre"
    }
}

</script>

<div class="ticker-el-expanded" in:fly="{{ x: 50, duration: 100, delay:100 }}" out:fly="{{ x: 50, duration: 100 }}">
    <div class="ticker-el-hours">
        <svg viewBox="0 0 {width} {height}" on:mouseleave={() => { highlight() }}>
            <rect class="quartile-marker" x="{width / 4}" width="1" height="100%" />
            <rect class="quartile-marker" x="{width / 4 * 2}" width="1" height="100%" />
            <rect class="quartile-marker" x="{width / 4 * 3}" width="1" height="100%" />
            {#each data as d, i}
            <rect class="hour-bar" width={width / 24} height="100%" x={width / 24 * i} 
                on:mouseover={() => { highlight(i) }} 
                on:focus={() => { highlight(i) }} />
            {/each}
            <polyline points={points}></polyline>
        </svg>
    </div>
    {#if hour != ""}
    <span class="border">{@html hour}{@html value}</span>
    {/if}
    <span class="border">
        Høyest: Kl <span class="red">{parseInt(data.findIndex(e => e == max)).toLocaleString('nb-NO', { minimumIntegerDigits: 2 })}-{(parseInt(data.findIndex(e => e == max)) + 1).toLocaleString('nb-NO', { minimumIntegerDigits: 2 })}</span> <strong>({max.toFixed(0)}</strong> øre)
    </span>
    <span>
        Lavest: Kl <span class="green">{parseInt(data.findIndex(e => e == min)).toLocaleString('nb-NO', { minimumIntegerDigits: 2 })}-{(parseInt(data.findIndex(e => e == min)) + 1).toLocaleString('nb-NO', { minimumIntegerDigits: 2 })}</span> <strong>({min.toFixed(0)}</strong> øre)
    </span>
</div>

<style>
.ticker-el-expanded {
    display: inline-flex;
    align-items: center;
    height: 100%;
}
.ticker-el-hours {
    margin-right: 5px;
}
.border {
    margin-right: 8px;
    padding-right: 8px;
    border-right: 1px solid #ccc;
}
svg {
    height: 20px;
    display: block;
    width: auto;
}
polyline {
    stroke: black;
    stroke-width: 1.5px;
    stroke-linecap: round;
    stroke-linejoin: round;
    fill: none;
}
rect.quartile-marker {
    fill: #ccc;
}
rect.hour-bar {
    fill: transparent;
    cursor: pointer;
    outline: none;
}
rect.hour-bar:hover {
    fill: #fcda51;
    opacity: .5;
}
.red {
    color: #d1002d;
    font-weight: bold;
}
.green {
    color: #406619;
    font-weight: bold;
}
</style>