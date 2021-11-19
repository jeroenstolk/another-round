<script>
    import BeerCard from "./beer-card.svelte";
    import Loading from "./UI/loading.svelte";

    let beers = Promise.resolve([]);

    async function fetchBeers(url) {
        const response = await fetch(url);

        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }

        beers = await response.json();
    }

    const beerRandom = () =>
        fetchBeers("https://api.punkapi.com/v2/beers/random");
    const beerAll = () => fetchBeers("https://api.punkapi.com/v2/beers");
    const beerAdd = () => fetchBeers("https://api.punkapi.com/v2/beers");

    beerAll();
</script>

<!-- Stop hitting GitHub on every source edit -->
<button on:click={beerAll}>All beers</button>
<button on:click={beerAdd}>Add a beer</button>
<button on:click={beerRandom}>Random beer</button>

{#await beers}
    <Loading />
{:then beers}
    <div class="container">
        <Loading />
        {#each beers as { id, name, image_url } (id)}
            <BeerCard {name} {image_url} {id} />
        {/each}
    </div>
{:catch error}
    <p style="color: red">{error.message}</p>
{/await}

<style>
    button {
        border-radius: 5px;
        background-color: #f3e803;
        color: #000;
        padding: 0.65em 1.1em;
        font-size: 1.2rem;
        cursor: pointer;
    }
    .container {
        display: flex;
        flex-direction: row;
        width: 100%;
        flex-wrap: wrap;
    }
</style>
