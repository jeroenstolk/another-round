<script>
	import Beers from "./Beers/beer-catalogue.svelte";
	import BeerDetails from "./Beers/beer-details.svelte";
	import Header from "./UI/header.svelte";
	import Footer from "./UI/footer.svelte";
	import Loading from "./UI/loading.svelte";

	let detailsView = false;
	let loading = true;
	let selectedBeer = 0;
	let beersData = Promise.resolve([]);
	let response = [];

	async function fetchBeers(selectedBeer) {
		if (detailsView) {
			response = await fetch(
				`https://api.punkapi.com/v2/beers/${selectedBeer}`
			);
		} else {
			response = await fetch("https://api.punkapi.com/v2/beers");
		}

		if (!response.ok) {
			throw new Error(`HTTP error! status: ${response.status}`);
		}

		beersData = await response.json();
		setTimeout(function () {
			loading = true;
		}, 3000);
	}

	function handleRequestSeeDetails(event) {
		detailsView = true;
		selectedBeer = event.detail.id;
		fetchBeers(selectedBeer);
	}

	function handleBeerAll() {
		detailsView = false;
	}
</script>

<Header />
{#if detailsView}
	<nav>
		<button on:click={handleBeerAll}>Back to all beers</button>
	</nav>
{/if}
<section class="main">
	{#await beersData}
		{#if loading}
			<Loading />
		{/if}
	{:then beersData}
		{#if detailsView}
			{#each beersData as item (item.id)}
				<BeerDetails {...item} />
			{/each}
		{:else}
			<Beers on:seeDetails={handleRequestSeeDetails} />
		{/if}
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}<!--  -->
	<Footer />
</section>

<style>
	.main {
		text-align: center;
		margin: 0 auto;
	}

	nav {
		text-align: center;
	}
</style>
