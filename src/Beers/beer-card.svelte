<script>
    import { createEventDispatcher } from "svelte";

    export let name;
    export let image_url;
    export let id;
    export let added;
    export let image;

    const dispatch = createEventDispatcher();

    function handleSeeDetails() {
        dispatch("seeDetails", {
            id: id,
        });
    }
</script>

<div class="item {added ? 'added' : ''}">
    <img src={added ? image : image_url} alt={name} {id} />
    <h3>{name}</h3>
    <button on:click={handleSeeDetails}>Read more</button>
</div>

<style>
    img {
        max-height: 300px;
    }
    h3 {
        color: #fff;
        margin:  1em 0;
    }
    button {
        align-self: center;
        margin-top: auto;
    }
    .item {
        display: flex;
        flex-direction: column;
        flex-basis: max-content;
        flex: 1 1 auto;
        background: radial-gradient(#75e4de, #04c1e1);
        border: 1px solid #fff;
        padding: 1em;
    }
    .item img {
        width: initial;
        align-self: center;
    }
    .added {
        background: radial-gradient(#b7ebb9, #75e4de);
    }
</style>
