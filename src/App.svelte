<script>
	import { onMount } from "svelte";
	import Icon from "./components/icon.svelte";
	import Loading from "./components/Loading.svelte";

    let state_loading = false
    let actual_load = false
    let result
    let search_stuff = ""
    const fetchdata = async () => {
        actual_load = true
        const test = await fetch("https://db.ygoprodeck.com/api/v7/cardinfo.php?fname="+search_stuff)
        result = await test.json()
        console.log(result)
        state_loading = true
        actual_load = false
    }
</script>
<span>Card Name search:</span>
<input type="text" name="" id="" bind:value={search_stuff} placeholder="type here">
<button on:click={fetchdata}>fetch data</button>
{#if state_loading}
        {#each result["data"] as d}
    {#if d["name"].indexOf(search_stuff) != -1}
    <div class="grid">
            <img src={d["card_images"][0]["image_url"]} alt="">
            <div class="info">
                {#if d["card_prices"][0]["amazon_price"] !== undefined}
                    <span><Icon icon="sell"/>Card price(amazon): ${d["card_prices"][0]["amazon_price"]}</span>
                {/if}
                {#if d["card_prices"][0]["ebay_price"] !== undefined}
                    <span><Icon icon="sell"/>Card price(ebay): ${d["card_prices"][0]["ebay_price"]}</span>
                {/if}
                <span><Icon icon="Badge"/>Card Name: {d["name"]}</span>
                <span><Icon icon="Ink_pen"/>Card Type: {d["type"]}</span>
                <span><Icon icon="Crop_free"/>Card Frame: {d["frameType"]}</span>
                {#if d["atk"] !== undefined}
                    <span><Icon icon="swords"/>Card attack: {d["atk"]}</span>
                {/if}
                {#if d["def"] !== undefined}
                    <span><Icon icon="shield"/>Card defence: {d["def"]}</span>
                {/if}
                {#if d["race"] !== undefined}
                    <span><Icon icon="Accessible_Forward"/>Card race: {d["race"]}</span>
                {/if}
                {#if d["attribute"] !== undefined}
                    <span>Card attribute: {d["attribute"]}</span>
                {/if}
                {#if d["archetype"] !== undefined}
                    <span>Card archetype: {d["archetype"]}</span>
                {/if}
                {#if d["level"] !== undefined}
                    <span>Card level: {d["level"]}</span>
                {/if}
                <span>Card description: {d["desc"]}</span>
            </div>
    </div>
    {/if}
        {/each}
{/if}
{#if actual_load == true}
    <Loading />
{/if}
<style>
    .grid{
        display: grid;
        grid-template-columns: auto auto;
        border-color: red;
        border-width: 10px;
        border-style: solid;
        margin-top: 5px;
        margin-bottom: 5px;
    }
    .info {
        display: grid;
        grid-template-columns: auto;
        border-color: orange;
        border-width: 10px;
        border-style: solid;
        margin: 5px;
        padding: 10px;
    }
</style>
