<header>
    <section class="topBar">Another round</section>
    <section class="intro">
        <h1>Check out the world's finest beers!</h1>
        <h3>
            Have fun exploring these fine offers. Also, you can add your own,
            and have others enjoy reading about them as well.
        </h3>
    </section>
</header>

<style>
    header {
        display: flex;
        flex-direction: column;
    }
    section.topBar {
        display: flex;
        flex-direction: row;
        background: #000;
        color: #fff;
        width: 100%;
        padding: 2em;
    }
    section.intro {
        display: flex;
        flex-direction: column;
        justify-content: center;
        text-align: center;
        align-content: center;
        padding: 2em;
    }
    h3 {color: #000}
    @media (max-width: 1000px) {
        h1 {font-size: 3rem;}
        h3 {font-size: 2.5rem}
    }
</style>
