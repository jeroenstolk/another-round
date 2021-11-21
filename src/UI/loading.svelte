<div>
    <img src="./images/beerLoader.png" alt="Beer loader" />
    <p>Have a beer while you wait!</p>
</div>

<style>
    div {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
    p {
        text-align: center;
    }
    img {
        height: 8em;
        align-self: center;

        animation: spin 1s linear infinite;
    }

    @keyframes spin {
        100% {
            -webkit-transform: rotate(360deg);
            transform: rotate(360deg);
        }
    }
</style>
