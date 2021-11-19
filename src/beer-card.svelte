<script>
    import Lazy from "svelte-lazy";
    export let name;
    export let image_url;
    export let id;
</script>

<div class="item">
    <img src={image_url} alt={name} {id} />
    <h3>{name}</h3>
    <button>Read more</button>
</div>

<style>
    img {
        max-height: 300px;
    }
    h3 {
        color: #fff;
    }
    button {
        align-self: center;
    }
    .item {
        display: flex;
        flex-direction: column;
        flex-basis: max-content;
        flex: 1 1 auto;
        width: calc(100% / 3);
        background: radial-gradient(#75e4de, #04c1e1);
        border: 1px solid #fff;
        padding: 1em;
    }
    .item img {
        width: initial;
        align-self: center;
    }
</style>
