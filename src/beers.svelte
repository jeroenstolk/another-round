<script>
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
</script>

<!-- Stop hitting GitHub on every source edit -->
<button on:click={beerAll}>All beers</button>
<button on:click={beerRandom}>Random beer</button>

{#await beers}
    <p>...waiting</p>
{:then beers}
    <div class="container">
        {#each beers as { id, name, image_url } (id)}
            <div class="item">
                <h3>{name}</h3>
                <img src={image_url} alt={id} />
            </div>
        {/each}
    </div>
{:catch error}
    <p style="color: red">{error.message}</p>
{/await}

<style>
    img {
        max-height: 300px;
    }
    button {
        border-radius: 3px;
        background-color: blue;
        color: cornsilk;
        padding: 0.5em;
    }
    .container {display: flex; flex-direction: row; width: 100%; flex-wrap: wrap;}
    .item {flex-basis: max-content; flex: 1 1 auto; width: 30vw}
</style>
