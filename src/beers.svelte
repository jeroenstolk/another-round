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

    beerAll();
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
                <img src={image_url} alt={id} />
                <h3>{name}</h3>
                <button>Read more</button>
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
    h3 {
        color: #fff;
    }
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
    .item {
        flex-basis: max-content;
        flex: 1 1 auto;
        width: calc(100% / 3);
        background: radial-gradient(#75e4de, #04c1e1);
        border: 1px solid #fff;
    }
</style>
