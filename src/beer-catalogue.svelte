<script>
    import BeerCard from "./beer-card.svelte";
    import Loading from "./UI/loading.svelte";
    import { gsap } from "gsap";
    import ScrollTrigger from "gsap/ScrollTrigger";
    import { onMount } from "svelte";

    let gsapCard;

    const trigger = () => {
        gsap.registerPlugin(ScrollTrigger);
        gsap.from(".container", {
            scrollTrigger: gsapCard,
            opacity: 0,
            duration: 2
        });
        gsap.to(".container", {
            scrollTrigger: gsapCard,
            opacity: 1,
            duration: 2
        });
    };

    let beers = Promise.resolve([]);

    async function fetchBeers(url) {
        const response = await fetch(url);

        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        } else {
            trigger();
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

<div class="testing">testing</div>

{#await beers}
    <Loading />
{:then beers}
    <div class="container" bind:this={gsapCard}>
        <Loading />
        {#each beers as { id, name, image_url } (id)}
            <div class="hidden">
                <BeerCard {name} {image_url} {id} />
            </div>
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
    .hidden {
        opacity: 1;
    }
</style>
