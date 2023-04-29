<script>
	import { onMount } from 'svelte';
	import axios from 'axios';

	let isError = false;

	/* 	const credentials = {
		email: 'kevinlhall72@gmail.com',
		password: 'SecretPassword'
	}; */

	/**
	 * @type {any}
	 */
	let quotes = [];
	/**
	 * @type {any}
	 */
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
		const auth_instance = axios.create({
			headers: {
				'X-Requested-With': 'XMLHttpRequest',
				Accept: 'application/json',
				'Access-Control-Allow-Origin': '*',
				'Access-Control-Allow-Methods': 'GET,PUT,POST,DELETE,OPTIONS'
			},
			baseURL: import.meta.env.VITE_LOCALHOST
		});

		const base_instance = axios.create({
			headers: {
				'X-Requested-With': 'XMLHttpRequest',
				Accept: 'application/json',
				'Access-Control-Allow-Origin': '*',
				'Access-Control-Allow-Methods': 'GET,PUT,POST,DELETE,OPTIONS'
			},
			baseURL: import.meta.env.VITE_LOCALHOST
		});

		base_instance.defaults.withCredentials = true;
		auth_instance.defaults.withCredentials = true;

		// This is for a frontend handling the Login page; not needed here because it is scaffolded in Jetstream
		// Sanctum takes care of the SPA cookies for auth as long as it is same root domain
		/* base_instance.get('sanctum/csrf-cookie').then((response) => {
			let cookies = document.cookie.split('=');
			let token = cookies[1];
			console.log(token, cookies, document.cookie);
			//Login...
			const res2 = auth_instance.post(`/login`, credentials);
			console.log(res2, 'one');
		}); */

		data = await fetch('https://api.coindesk.com/v1/bpi/currentprice.json').then((x) => x.json());

		try {
			// const restoo = await base_instance.get('sanctum/csrf-cookie').then((response) => {
			// 	let cookies = document.cookie.split('=');
			// 	let token = cookies[1];
			// 	console.log(token, cookies, document.cookie);
			// 	// Login...
			// });

			// const res = await axios.get(`https://famous-quotes4.p.rapidapi.com/random`, {
			// 	headers: {
			// 		'x-rapidapi-host': 'famous-quotes4.p.rapidapi.com',
			// 		'x-rapidapi-key': API_KEY
			// 	},
			// 	params: { category: 'all', count: '10' }
			// });
			const res = await axios.get(`/api/world`, {
				headers: {},
				baseURL: import.meta.env.VITE_LOCALHOST
			});
			world = res.data;
			console.log(world);
			const resevents = await auth_instance.get(`/api/my/events`);
			console.log(resevents.data, 'two');
			const res2events = await auth_instance.get(`/api/user`);
			console.log(res2events.data, 'two');
			// const wres = await axios.get(`/api/events`, {
			// 	headers: {
			// 		Authorization: 'Bearer ' + import.meta.env.VITE_BTOKEN_DEV
			// 	},
			// 	baseURL: import.meta.env.VITE_LOCALHOST
			// });
			// console.log(wres.data);

			// const pres = await axios.get(`/players`, {
			// 	headers: {
			// 		Authorization: 'Bearer ' + import.meta.env.VITE_BTOKEN_DEV
			// 	},
			// 	baseURL: import.meta.env.VITE_PROD_W3X
			// });
			// console.log(pres.data);

			// const plyres2 = axios.get(`/players`, {
			// 	headers: {
			// 		Accept: 'application/json',
			// 		'Access-Control-Allow-Origin': '*'
			// 	},
			// 	baseURL: import.meta.env.VITE_LOCAL_W3X
			// });
			// console.log(plyres2);

			// axios.get('http://w3xfc.klh/sanctum/csrf-cookie').then((response) => {
			// 	console.log(response, 'done2');
			// 	let cookies = document.cookie.split('=');
			// 	let token = cookies[1];
			// 	console.log(token, cookies, document.cookie);

			// 	const plyres = axios.get(`/players`, {
			// 		headers: {
			// 			Accept: 'application/json',
			// 			'Access-Control-Allow-Origin': '*',
			// 			'X-XSRF-TOKEN': token
			// 		},
			// 		baseURL: import.meta.env.VITE_LOCAL_W3X
			// 	});
			// 	console.log(plyres);

			// 	const eventres = axios.get(`/events`, {
			// 		headers: {
			// 			Accept: 'application/json',
			// 			'Access-Control-Allow-Origin': '*',
			// 			'X-XSRF-TOKEN': token
			// 		},
			// 		baseURL: import.meta.env.VITE_LOCAL_W3X
			// 	});
			// 	console.log(eventres);
		} catch (e) {
			error = e;
			isError = true;
			console.log(
				error.response.status,
				error.response.data.message,
				error.response.statusText,
				':',
				error.message
			);
		}
	});
</script>

{#if isError}
	<pre style="color:red">{error.response.data.message} {error.response
			.statusText}: {error.message}</pre>
{/if}

<svelte:head>
	<title>About</title>
	<meta name="description" content="About this app" />
</svelte:head>

<div class="text-column">
	<h1>A {world ? 'Hello ' + world : ''} Page for this demo app</h1>
	<div>quotes here</div>
	{#if data}
		<pre>
			{JSON.stringify(data, null, 2)}
		</pre>
	{/if}
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
