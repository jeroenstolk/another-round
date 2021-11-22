<script>
    export let name;
    export let image_url;
    export let added;
    export let tagline;
    export let description;
    export let first_brewed;
    export let brewers_tips;
    export let contributed_by;
    export let food_pairing;
</script>

<div class="item {added ? 'added' : ''}">
    <div class="detailsContainer">
        <div class="beerImage">
            <img src={image_url} alt={name} />
        </div>
        <div class="beerDetails">
            <h2>{name}</h2>
            <h3>{tagline}</h3>
            <p>{description}</p>
            <p style="font-weight: bold;">Some details and fun facts!</p>
            <ul>
                <li>First brewed: {first_brewed}</li>
                <li>Brewer's tips: {brewers_tips}</li>
                <li>Contributed by: {contributed_by}</li>
                <li>Goes nice with the following dishes:</li>
                <ul>
                    {#each food_pairing as food}
                        <li>{food}</li>
                    {/each}
                </ul>
            </ul>
        </div>
    </div>
</div>

<style>
    img {
        max-height: 500px;
    }
    h3 {
        color: #fff;
    }

    .item {
        display: flex;
        flex-direction: column;
        flex-basis: max-content;
        flex: 1 1 auto;
        width: calc(100% / 3);
        background: radial-gradient(#75e4de, #04c1e1);
        border: 1px solid #fff;
        padding: 3em;
        width: 100%;
    }
    .item img {
        width: initial;
        align-self: center;
    }
    .added {
        background: radial-gradient(#b7ebb9, #75e4de);
    }
    .detailsContainer {
        display: flex;
        flex-direction: row;
    }
    @media (max-width: 1000px) {
        .detailsContainer {
            flex-direction: column;
        }
    }
    .beerDetails,
    .beerImage {
        width: 50%;
    }

    @media (max-width: 1000px) {
        .beerDetails,
        .beerImage {
            width: 100%;
            flex-wrap: wrap;
        }
        .beerImage {margin-bottom: 2em;}
    }

    .beerDetails {
        text-align: start;
        background: #fff;
        border-radius: 0.5em;
        padding: 1.5em;
        padding-bottom: 3em;
    }
    h3 {
        color: #000;
    }
</style>
