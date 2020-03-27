<script context="module">
	export async function preload({ params, query }) {
      return {params, query}
	}
</script>

<script>
	export let params;
	export let query;
	let queryKeysArray;

	$: console.log('$$$$ params', params)
	$: console.log('$$$$ query', query)

	$: queryKeysArray = Object.keys(query)
</script>

<svelte:head>
	<title>{params}</title>
</svelte:head>

<div>This my Confidential Page </div>

<br />
<br />

<div>
	<div>URL Params -</div>
	<ul>
		{#each params.param as param}
			<li>{param}</li>
		{/each}
</ul>
</div>

<div>
	<div>URL Query -</div>
	<ul>
		{#each queryKeysArray as queryKey}
			<li>{query[queryKey]}</li>
		{/each}
	</ul>
</div>