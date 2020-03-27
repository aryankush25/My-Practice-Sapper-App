<script context="module">
	export async function preload({ params, query }) {
      return {params, query}
	}
</script>

<script>
	export let params;
	export let query;
	let queryKeysArray;
	let title;

	const getTitle = (param) => {
		let temp = '';

		for (let i = 0; i < param.length; i++) {
			const element = param[i];
			if (i === 0) {
				temp = temp  + element
			} else {
				temp = temp + '_' + element
			}
		}

		return temp
	}

	$: console.log('$$$$ params', params)
	$: console.log('$$$$ query', query)

	$: queryKeysArray = Object.keys(query)

	$: title = getTitle(params.param)
	
</script>

<svelte:head>
	<title>{title}</title>
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