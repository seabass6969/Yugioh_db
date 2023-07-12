<script>
	import { onMount } from "svelte";
	import Icon from "./components/icon.svelte";

    let banana = "mum"
    let luke_broken = ["arm", "legs", "Jethro"]
    // Look at me this is comment
    let me = "me"
    const changetext = () => {
        me = "Yourself"
    }
    let state_loading = false
    let result
    let search_stuff = ""
    const fetchdata = async () => {
        const test = await fetch("https://db.ygoprodeck.com/api/v7/cardinfo.php?fname="+search_stuff)
        result = await test.json()
        console.log(result)
        state_loading = true
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
                <span><Icon icon="sell"/>Card price(amazon): ${d["card_prices"][0]["amazon_price"]}</span>
                <span><Icon icon="sell"/>Card price(ebay): ${d["card_prices"][0]["ebay_price"]}</span>
                <span><Icon icon="Badge"/>Card Name: {d["name"]}</span>
                <span><Icon icon="Ink_pen"/>Card Type: {d["type"]}</span>
                <span><Icon icon="Crop_free"/>Card Frame: {d["frameType"]}</span>
                <span><Icon icon="swords"/>Card attack: {d["atk"]}</span>
                <span><Icon icon="shield"/>Card defence: {d["def"]}</span>
                <span><Icon icon="Accessible_Forward"/>Card race: {d["race"]}</span>
                <span>Card attribute: {d["attribute"]}</span>
                {#if d["archetype"] !== undefined}
                    <span>Card archetype: {d["archetype"]}</span>
                {/if}
                <span>Card level: {d["level"]}</span>
                <span>Card description: {d["desc"]}</span>
            </div>
    </div>
    {/if}
        {/each}
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
