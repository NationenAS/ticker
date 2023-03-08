<script>
import { onMount } from "svelte"
import Arrow from "./Arrow.svelte"

let data
let elRegion = "Øst"

onMount(async () => {
    fetch("https://ravarepriser.vercel.app/api/now")
    .then(r => r.json())
    .then(d => {
        data = d
    })
    .catch(e => console.log(e))
})

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
        Strøm ({elRegion}) <Arrow type="{data.symbols.Strøm[elRegion].change == 0 ? "neu" : data.symbols.Strøm[elRegion].change > 0 ? "pos" : "neg"}" /> <strong>{data.symbols.Strøm[elRegion].data[1].avg}</strong> øre
    </div>
    <div class="ticker-symbol">
        
    </div>
{/if}


</div>

<style>

.ticker {
    height: 100%;
    width: 100%;
    font-family: "Open Sans", sans-serif;
    font-size: 12px;
}
.ticker > div {
    display: inline-block;
    margin-right: 5px;
}
.ticker-title {
    text-transform: uppercase;
    font-weight: bold;
    color: #406619;
}

</style>
