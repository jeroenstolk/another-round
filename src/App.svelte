<script>
	import Beers from "./Beers/beer-catalogue.svelte";
	import Header from "./UI/header.svelte";
	import Footer from "./UI/footer.svelte";
</script>

<Header />
<section class="main">
	<Beers />
	<Footer />
</section>

<style>
	.main {
		text-align: center;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		.main {
			max-width: none;
		}
	}
</style>
