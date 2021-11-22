<script>
    import BeerCard from "./beer-card.svelte";
    import Loading from "../UI/loading.svelte";
    import { fade } from "svelte/transition";
    import { onMount } from "svelte";
    import { scale } from "svelte/transition";
    import { flip } from "svelte/animate";

    let visible = false;
    let showForm = false;

    onMount(async () => {
        visible = true;
    });

    var loading = true;
    let beers = Promise.resolve([]);

    let fName = "name here";
    let fDescription = "desc here";
    let fTagline = "tag here";
    let fImage = "";

    async function fetchBeers(url) {
        const response = await fetch(url);

        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }

        beers = await response.json();
    }

    const onFileSelected = (e) => {
        let image = e.target.files[0];
        let reader = new FileReader();
        reader.onload = function () {
            fImage = reader.result;
            console.log(fImage);
        };
        reader.readAsDataURL(image);
    };

    let fetchUrlAll = "https://api.punkapi.com/v2/beers";
    let fetchUrlRandom = "https://api.punkapi.com/v2/beers/random";

    const handleBeerRandom = () => fetchBeers(fetchUrlRandom);
    const handleBeerAll = () => fetchBeers(fetchUrlAll);
    const handleFormAddBeer = () =>
        showForm ? (showForm = false) : (showForm = true);

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
        console.log(beers);
        showForm = false;
    }

    let showNav = true;

    function handleRequestSeeDetails(event) {
        showNav = false;
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

{#if showNav}
    <nav>
        <button on:click={handleBeerAll}>All beers</button>
        <button on:click={handleFormAddBeer}>Add beer</button>
        <button on:click={handleBeerRandom}>Random beer</button>
    </nav>
{/if}

{#await beers}
    {#if loading}
        <Loading />
    {/if}
{:then beers}
    <div class="container" transition:fade>
        {#each beers as beer (beer.id)}
            <div
                class="beerCard"
                transition:scale
                animate:flip={{ duration: 300 }}
            >
                <BeerCard
                    name={beer.name}
                    image_url={beer.image_url}
                    id={beer.id}
                    added={beer.added}
                    image={beer.image}
                    on:seeDetails
                    on:seeDetails={handleRequestSeeDetails}
                />
            </div>
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
        background: #04c1e1;
    }
    .beerCard {
        display: flex;
        width: calc(100% / 4);
    }
    @media (max-width: 1000px) {
        .beerCard {
            width: calc(100% / 2);
        }
    }
    @media (max-width: 600px) {
        .beerCard {
            width: 100%;
        }
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
    @media (max-width: 600px) {
        .formAdd {
            width: 90%;
        }
    }
    .closeFormAdd {
        display: flex;
        justify-content: flex-end;
        cursor: pointer;
    }
</style>
