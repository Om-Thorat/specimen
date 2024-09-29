<script>
	import Header from './Header.svelte';
	import '../app.css';
</script>

<div class="app bg-[#151619]">
	<Header />

	<main>
		<slot />
	</main>
</div>
