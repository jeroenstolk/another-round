<script>
    import BeerCard from "./beer-card.svelte";
    import Loading from "../UI/loading.svelte";
    import { fade } from "svelte/transition";
    import { onMount } from "svelte";

    let visible = false;
    let showForm = false;

    onMount(async () => {
        visible = true;
    });

    var loading = false;
    let beers = Promise.resolve([]);

    let fName = "name here";
    let fDescription = "desc here";
    let fTagline = "tag here";
    let fImage = "";

    async function fetchBeers(url) {
        loading = true;
        const response = await fetch(url);

        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }

        beers = await response.json();
    }

    const onFileSelected = (e) => {
        let image = e.target.files[0];
        let reader = new FileReader();
        reader.readAsDataURL(image);
        console.log(reader.onload.result);
        console.log(reader.result);
    };

    let fetchUrlAll = "https://api.punkapi.com/v2/beers";
    let fetchUrlRandom = "https://api.punkapi.com/v2/beers/random";

    const handleBeerRandom = () => fetchBeers(fetchUrlRandom);
    const handleBeerAll = () => fetchBeers(fetchUrlAll);
    const handleFormAddBeer = () => (showForm ? (showForm = false) : (showForm = true));

    handleBeerAll();

    function submitHandle() {
        const newBeer = {
            id: beers.length + 1,
            name: fName,
            description: fDescription,
            tagline: fTagline,
            image: fImage,
            added: true,
        };

        beers = [newBeer, ...beers];
        showForm = false;
        console.log(beers);
    }
</script>

<div class={showForm ? "formAdd" : "formAdd hidden"}>
    <div class="closeFormAdd" on:click={handleFormAddBeer}>Luk</div>
    <form on:submit|preventDefault={submitHandle}>
        <label for="fName">Name:</label>
        <input type="text" id="fName" bind:value={fName} />
        <label for="fDescription">Description:</label>
        <textarea rows="3" id="fDescription" bind:value={fDescription} />
        <label for="fTagline">Tagline:</label>
        <input type="text" id="fTagline" bind:value={fTagline} />
        <label for="fImage">Tagline:</label>
        <input
            type="file"
            accept=".jpg, .jpeg, .png"
            id="fImage"
            on:change={(e) => onFileSelected(e)}
            bind:value={fImage}
        />
        <button type="submit">Add this beer</button>
    </form>
</div>

<nav>
    <button on:click={handleBeerAll}>All beers</button>
    <button on:click={handleFormAddBeer}
        >Add beer</button
    >
    <button on:click={handleBeerRandom}>Random beer</button>
</nav>

{#await beers}
    {#if loading}
        <Loading />
    {/if}
{:then beers}
    <div class="container" transition:fade>
        {#each beers as { id, name, image_url, added } (id)}
            <BeerCard {name} {image_url} {id} {added} />
        {/each}
    </div>
{:catch error}
    <p style="color: red">{error.message}</p>
{/await}

<style>
    .container {
        display: flex;
        flex-direction: row;
        width: 100%;
        flex-wrap: wrap;
    }
    .formAdd {
        position: absolute;
        margin-left: 50%;
        transform: translateX(-50%);
        min-width: 75%;
        background: #fff;
        box-shadow: 2px 5px 15px rgba(0, 0, 0, 0.3);
        padding: 2em;
    }
    .closeFormAdd {display: flex; justify-content: flex-end; }
</style>
