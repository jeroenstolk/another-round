<script>
    import { tweened } from "svelte/motion";
    import { bounceInOut } from "svelte/easing";

    function getSoundValues(total) {
        const barsData = [];
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
        <div
            class="soundbar"
            style="height: {Math.round($barsDataTween[index])}%"
        />
    {/each}
</div>

<style>
    .soundbars {
        display: flex;
        flex-direction: row;
        align-items: flex-end;
        height: 50px;
        gap: 4px;
    }
    :global(.soundbar) {
        width: 2%;
        background-color: #e5e5f7;
        background-size: 100% 10px;
        background-position: bottom;
        image-rendering: optimizeQuality;
        background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' height='50' width='100'%3E%3Crect width='100' height='25' x='0' y='0' style='fill:white;' /%3E%3Crect width='100' height='25' x='0' y='25' style='fill:black;' /%3E%3C/svg%3E");
    }
    .hidden {
        display: none;
    }
</style>
