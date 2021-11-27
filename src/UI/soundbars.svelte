<script>
    import { tweened } from "svelte/motion";
    import { bounceInOut } from "svelte/easing";

    function getSoundValues(total) {
        let barsData = [];
        for (let index = 0; index < total; index++) {
            barsData.push(Math.random() * 100);
        }
        return barsData;
    }

    const soundValues = getSoundValues(12);

    const barsDataTween = tweened(soundValues, {
        duration: 150,
        easing: bounceInOut,
    });

    setInterval(function () {
        barsDataTween.set(getSoundValues(12));
    }, 150);

</script>

<ul class="hidden">
    {#each soundValues as value, index}
        <li>{Math.round($barsDataTween[index])}</li>
    {/each}
</ul>
<div class="soundbars">
    {#each soundValues as value, index}
        <div class="soundbar" style="height: {Math.round($barsDataTween[index])}%" />
    {/each}
</div>

<style>
    .soundbars {
        display: flex;
        flex-direction: row;
        align-items: flex-end;
        height: 100px;
        gap: 0.2em;
    }
    :global(.soundbar) {
        width: 2%;
        background-color: #e5e5f7;
        background-image: linear-gradient(0deg, #000 50%, #fff 50%);
        background-size: 10px 10px;
        background-position: bottom;
        image-rendering: optimizeQuality;
    }
    .hidden {
        display: none;
    }
</style>
