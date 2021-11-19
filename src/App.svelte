<script>
	import Beers from "./beer-catalogue.svelte";
	import Header from "./UI/header.svelte";
</script>

<Header />
<section class="main">
	<Beers />
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
