<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}


<style>
 :global(body){
    background-color: #F6F9FD;
    margin: 1.50rem 0;
    padding: 0;
    line-height: 1.5;
    font-family: "Roboto", sans-serif;
    font-optical-sizing: auto;
    font-style: normal;
}
</style>
