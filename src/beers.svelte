<script>
    const fetchBeers = (async () => {
        const response = await fetch("https://api.punkapi.com/v2/beers");
        return await response.json();
    })();
    
</script>


{#await fetchBeers}
    <p>Loading beers...</p>
{:then beers}
    {#each beers as beer}
        <img src={beer.image_url} alt={beer.name} />
    {/each}
{/await}

<style>
</style>
