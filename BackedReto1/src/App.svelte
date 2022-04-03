<script>
	let count = 0;
	let gameId = 0;
	let goal = 0;

	let emoji = "";

	async function getEmojiItems() {
		var myHeaders = new Headers();

		var requestOptions = {
			method: "GET",
			redirect: "follow",
		};

		const response = await fetch(
			"https://w4390n0i.directus.app/items/Emojis",
			requestOptions
		);
		console.log(response);
		const result = response.json();

		/* Return result if successful
		 */
		if (response.ok) {
			return result;
		} else {
			console.log("error getEmojiItems");
		}
	}

	async function getNewMiningGame() {
		var requestOptions = {
			method: "POST",
			redirect: "follow",
		};

		const response = await fetch(
			"https://w4390n0i.directus.app/items/Games",
			requestOptions
		);

		const result = response.json();

		if (response.ok) {
			return result;
		} else {
			console.log("error getNewMiningGame");
		}
	}

	function incrementCount2() {
		if (count == goal) {
			return;
		}
		console.log("incrementCount2", gameId);
		var requestOptions = {
			method: "PATCH",
			body: JSON.stringify({
				current: count + 1,
			}),
			headers: {
				"Content-type": "application/json; charset=UTF-8",
			},
		};
		const response = fetch(
			"https://w4390n0i.directus.app/items/Games/" + gameId,
			requestOptions
		).then((response) => response.json());

		count = count + 1;

		// post to log count value (backend saves datetime automatically)

		let r = fetch("https://w4390n0i.directus.app/items/Mining", {
			method: "POST",
			body: JSON.stringify({
				game_id: gameId,
				value: count,
			}),
			headers: {
				"Content-type": "application/json; charset=UTF-8",
			},
		});
		if (count == goal) {
			emoji = "👑";
		}
	}

	function setGameValues(gameIdP, goalP) {
		console.log("new game: ", gameIdP);
		gameId = gameIdP;
		goal = goalP; 
		return "";
	}

	function setEmojiValues(emojiP) {
		emoji = emojiP;
		return "";
	}
	const promise = getEmojiItems();

	const promiseMiningGame = getNewMiningGame();

	import { fade, fly } from "svelte/transition";
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>
<main>
	{#await promiseMiningGame then game}
		{setGameValues(game.data.id, game.data.goal)}
	{/await}
	{#await promise}
		<p>Loading...</p>
	{:then items}
		<h1 in:fade={{ y: 200, duration: 2000 }}>{emoji}</h1>
		{setEmojiValues(
			items.data[Math.floor(Math.random() * items.data.length)].Emoji
		)}
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}

	<button on:click={incrementCount2}> ⛏️ </button>

	<div class="footer">
		<p>
			Solucion para Reto de <a
				href="https://github.com/malandrinersdev/backend-de-la-suerte"
			>
				Backend de la suerte</a
			>
			| Visita la comunidad
			<a href="https://danielprimo.io">DanielPrimo.io</a> para otros retos
			en pantuflas como este | Done with ❤️
		</p>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 8em;
		font-weight: 200;
	}

	p {
		font-size: 0.8em;
	}
	.footer {
		position: fixed;
		left: 10px;
		bottom: 5px;
		right: 10px;
		width: 95%;
		text-align: center;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
