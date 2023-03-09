<script>
import { onMount } from "svelte"
import { fly } from "svelte/transition"
import Hours from "./Hours.svelte"
import Symbol from "./Symbol.svelte"

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

<div class=ticker-track>

{#if data === undefined }
    <div>Laster..</div>
{:else}
    <div class=ticker-title>Energi</div>
    <Symbol title="Strøm ({elRegion})" data={data.symbols.Strøm[elRegion]} border=false />
    <div class="ticker-toggle">
        <div on:click={toggle} on:keypress={toggle}>{ !expand ? "Se mer" : "Lukk" }</div>
    </div>
    {#if expand}
    <Hours data={data.symbols.Strøm[elRegion].data[1].hours} />
    {:else}
    <div class=ticker-not-el in:fly|local="{{ x: 50, duration: 100, delay:100 }}" out:fly="{{ x: 50, duration: 100 }}">
        <Symbol title="Strøm (Nord)" data={data.symbols.Strøm.Nord} border=true />
        <Symbol title="Råolje" data={data.symbols.Råolje} border=true />
        <div class="ticker-title" style="margin-left: 10px;">Råvarer</div>
        <Symbol title="Soya" data={data.symbols.Soya} border=false />
    </div>
    {/if}
{/if}


</div>

<style>

.ticker-track {
    height: 100%;
    width: 100%;
    font-family: "Open Sans", sans-serif;
    font-size: 12.5px;
    overflow-x: scroll;
    scrollbar-width: none;
    touch-action: pan-x;
    white-space: nowrap;
    line-height: 1;
    display: flex;
}
.ticker-track::-webkit-scrollbar {
    display: none;
}
.ticker-title,
.ticker-toggle,
.ticker-not-el {
    display: inline-flex;
    align-items: center;
    height: 100%;
}
.ticker-title {
    margin-right: 8px;
    text-transform: uppercase;
    font-weight: bold;
    color: #406619;
}
.ticker-toggle div {
    cursor: pointer;
    margin-right: 8px;
    font-size: .8em;
    text-transform: uppercase;
    padding: 2px 3px;
    line-height: 1;
    border: 1px solid #aaa;
    background: #eee;
}
.ticker-toggle div:hover {
    background: #ddd;
}

</style>
