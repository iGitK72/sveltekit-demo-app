<script>
	import { onMount } from 'svelte';
	import axios from 'axios';

	let quotes = [];
	let error = null;
	/**
	 * @type {any}
	 */
	let world;
	/**
	 * @type {any}
	 */
	let data;
	onMount(async () => {
		data = await fetch('https://api.coindesk.com/v1/bpi/currentprice.json').then((x) => x.json());

		try {
			// const res = await axios.get(`https://famous-quotes4.p.rapidapi.com/random`, {
			// 	headers: {
			// 		'x-rapidapi-host': 'famous-quotes4.p.rapidapi.com',
			// 		'x-rapidapi-key': API_KEY
			// 	},
			// 	params: { category: 'all', count: '10' }
			// });
			const res = await axios.get(`/world`, {
				headers: {
					'Access-Control-Allow-Origin': 'http://localhost:8000',
					'Access-Control-Allow-Methods': 'GET,PUT,POST,DELETE,OPTIONS'
				},
				baseURL: import.meta.env.VITE_MY_VAR
			});
			world = res.data;
			//console.log(world);
		} catch (e) {
			error = e;
		}
	});
</script>

<pre>
	{JSON.stringify(data, null, 2)}
</pre>

<svelte:head>
	<title>About</title>
	<meta name="description" content="About this app" />
</svelte:head>

<div class="text-column">
	<h1>Hello {world} Page for this demo app</h1>
	<div>quotes here</div>

	<p>
		This is a <a href="https://kit.svelte.dev">SvelteKit</a> app. You can make your own by typing the
		following into your command line and following the prompts:
	</p>

	<pre>npm create svelte@latest</pre>

	<p>
		The page you're looking at is purely static HTML, with no client-side interactivity needed.
		Because of that, we don't need to load any JavaScript. Try viewing the page's source, or opening
		the devtools network panel and reloading.
	</p>

	<p>
		The <a href="/sverdle">Sverdle</a> page illustrates SvelteKit's data loading and form handling. Try
		using it with JavaScript disabled!
	</p>
</div>
