<script>
import { onMount } from "svelte"
import { fly } from "svelte/transition";
import Arrow from "./Arrow.svelte"
import Hours from "./Hours.svelte"

let data
let elRegion = "Øst"
let expand = false

onMount(async () => {
    fetch("https://ravarepriser.vercel.app/api/now")
    .then(r => r.json())
    .then(d => {
        data = d
    })
    .catch(e => console.log(e))
})

function toggle() {
    expand = !expand
}

</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="true">
	<link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;700&display=swap" rel="stylesheet">
</svelte:head>

<div class=ticker>

{#if data === undefined }
    <div>Laster..</div>
{:else}
    <div class=ticker-title>Energi</div>
    <div class=ticker-symbol>
        Strøm ({elRegion}) 
        <Arrow type="{data.symbols.Strøm[elRegion].change == 0 ? "neu" : data.symbols.Strøm[elRegion].change > 0 ? "pos" : "neg"}" /> <strong>{data.symbols.Strøm[elRegion].data[1].avg}</strong> øre
    </div>
    <div on:click={toggle} on:keypress={toggle} class="toggle">{ !expand ? "Se mer" : "Lukk" }</div>
    {#if expand}
    <div in:fly="{{ x: 50, duration: 300, delay:200 }}" out:fly="{{ x: 50, duration: 200 }}">
        <Hours data={data.symbols.Strøm[elRegion].data[1].hours} />
    </div>
    {:else}
    <div class=ticker-not-el in:fly="{{ x: 50, duration: 200, delay:200 }}" out:fly="{{ x: 50, duration: 200 }}">
        <div class="ticker-symbol border">Strøm (Nord) <Arrow type="{data.symbols.Strøm.Nord.change == 0 ? "neu" : data.symbols.Strøm.Nord.change > 0 ? "pos" : "neg"}" /> <strong>{data.symbols.Strøm.Nord.data[1].avg}</strong> øre</div>
        <div class="ticker-symbol border">Råolje <Arrow type="{data.symbols.Råolje.change == 0 ? "neu" : data.symbols.Råolje.change > 0 ? "pos" : "neg"}" /> $<strong>{data.symbols.Råolje.now}</strong></div>
        <div class=ticker-title>Råvarer</div>
        <div class=ticker-symbol>Soya <Arrow type="{data.symbols.Soya.change == 0 ? "neu" : data.symbols.Soya.change > 0 ? "pos" : "neg"}" /> $<strong>{data.symbols.Soya.now}</strong></div>
    </div>
    {/if}
{/if}


</div>

<style>

.ticker {
    height: 100%;
    width: 100%;
    font-family: "Open Sans", sans-serif;
    font-size: 12.5px;
    display: flex;
    align-items: center;
}
.ticker-title {
    margin-right: 8px;
    text-transform: uppercase;
    font-weight: bold;
    color: #406619;
}
.ticker-symbol {
    margin-right: 8px;
}
.toggle {
    cursor: pointer;
    margin-right: 8px;
    font-size: .8em;
    text-transform: uppercase;
    padding: 2px 3px;
    line-height: 1;
    border: 1px solid #aaa;
    background: #eee;
}
.toggle:hover {
    background: #ddd;
}
.border {
    padding-left: 8px;
    border-left: 1px solid #ccc;
}
.ticker-not-el {
    display: flex;
    align-items: center;
    flex: 1;
}

</style>
