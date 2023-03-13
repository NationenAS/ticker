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

let customData= {
    kpi: {
        now: 6.3,
        change: -0.75
    },
    arb: {
        now: 3.4,
        change: 0
    }
}

</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="true">
	<link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;700&display=swap" rel="stylesheet">
</svelte:head>

<div class=ticker-track>

{#if data === undefined }
    <div>Laster...</div>
{:else}
    <div class=ticker-title>Strøm</div>
    <Symbol title="{elRegion}" data={data.symbols.Strøm[elRegion]} border=false description="Gjennomsnitt i dag per kWh uten moms." />
    <div class="ticker-toggle">
        <div on:click={toggle} on:keypress={toggle}>{ !expand ? "Se mer" : "Lukk" }</div>
    </div>
    {#if expand}
    <Hours data={data.symbols.Strøm[elRegion].data[1].hours} />
    {:else}
    <Symbol title="Nord" data={data.symbols.Strøm.Nord} border=true description="Gjennomsnitt i dag per kWh uten moms." />
    <div class="ticker-title" style="margin-left: 10px;" in:fly|local="{{ x: 50, duration: 100, delay:100 }}" out:fly="{{ x: 50, duration: 100 }}">Råvarer</div>
    <Symbol title="Råolje" data={data.symbols.Råolje} border=false description="Brent olje per fat." />
    <Symbol title="Gass" data={data.symbols.Gass} border=true description="Naturgass (TTF, leveranse i april), per MWh." />
    <Symbol title="Soya" data={data.symbols.Soya} border=true description="Soyabønner per tonn." />
    <div class="ticker-title" style="margin-left: 10px;" in:fly|local="{{ x: 50, duration: 100, delay:100 }}" out:fly="{{ x: 50, duration: 100 }}">Makro</div>
    <Symbol title="KPI" data={customData.kpi} border=false description="Konsumprisindeks endring feb 22 - feb 23." />
    <Symbol title="Ledighet" data={customData.arb} border=false description="Arbeidsledige i prosent av arbeidsstyrken jan 22." />
    {/if}
{/if}


</div>

<style>

.ticker-track {
    height: 100%;
    width: 100%;
    font-family: "Open Sans", sans-serif;
    font-size: 13px;
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
.ticker-toggle {
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
    padding: 2.5px 3px;
    line-height: 1;
    border: 1px solid #aaa;
    background: #eee;
}
.ticker-toggle div:hover {
    background: #ddd;
}

</style>
