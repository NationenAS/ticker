<script>
import Arrow from "./Arrow.svelte"
import { fly } from "svelte/transition"

export let title
export let data
export let border

</script>

<div class="ticker-symbol" class:border="{border == "true"}" in:fly|local="{{ x: 50, duration: 100, delay:100 }}" out:fly="{{ x: 50, duration: 100 }}">
    <span class="ticker-symbol-title">{title}</span>
    <Arrow type="{data.change == 0 ? "neu" : data.change > 0 ? "pos" : "neg"}" />
    {#if title == "Soya" || title == "Råolje"}
    $<span class="ticker-symbol-value"><strong>{parseFloat(data.now).toLocaleString("nb-NO")}</strong></span>
    {:else}
    <span class="ticker-symbol-value"><strong>{parseFloat(data.avg).toFixed(0)}</strong> øre</span>
    {/if}
</div>

<style>
.ticker-symbol {
    display: inline-flex;
    align-items: center;
    margin-right: 8px;
    height: 100%;
}
.ticker-symbol-title {
    margin-right: 5px;
}
.ticker-symbol-value {
    margin-left: 1px;
}
.border {
    padding-left: 8px;
    border-left: 1px solid #ccc;
}
</style>