<script lang="ts">
	import Calculation from './pages/Calculation.svelte';
	import About from './pages/About.svelte';
	import Sidebar from './components/Sidebar.svelte';

	import { fade } from 'svelte/transition';
	import { afterUpdate } from 'svelte';
	import Tab from './tab';

	const FADE_DURATION = 200;

	let activeTab: Tab = Tab.calculator;

	function checkHash() {
		const hash = window.location.hash.substring(1);
		if (!Object.keys(Tab).includes(hash)) return;

		activeTab = Tab[hash as keyof typeof Tab] ?? Tab.calculator;
	}
	checkHash();

	// update hash
	afterUpdate(() => {
		window.location.hash = `#${Tab[activeTab] ?? ''}`;
	});
</script>

<svelte:window on:hashchange={checkHash} />

<div class="wrapper">
	<aside>
		<Sidebar bind:activeTab />
	</aside>

	<main>
		{#if activeTab === Tab.calculator}
			<div in:fade={{ duration: FADE_DURATION }}>
				<Calculation />
			</div>
		{/if}
		{#if activeTab === Tab.about}
			<div in:fade={{ duration: FADE_DURATION }}>
				<About />
			</div>
		{/if}
	</main>
</div>

<style lang="scss">
	@use './scss/abstracts' as a;

	.wrapper {
		display: grid;
		grid-template-areas: 'sidebar main';
		grid-template-columns: clamp(5vw, 18rem, 25vw) 1fr;

		overflow: hidden;
		height: 100vh;

		aside {
			grid-area: sidebar;
			overflow: auto;
		}

		main {
			grid-area: main;
			overflow: auto;

			width: clamp(30rem, 75rem, 95%);
			padding: 2rem;
		}
	}
</style>
