
<script>
	// import HttpError from '$lib/HttpError'
	async function getEmojiItems() {
		var myHeaders = new Headers();
		
		var requestOptions = {
			method: 'GET',
			// headers: myHeaders,
			redirect: 'follow'
			};

			
		const response = await fetch("https://w4390n0i.directus.app/items/Emojis", requestOptions)
		console.log(response);
		const result = response.json()

		/* Return result if successful
		*/
		if(response.ok) {
			console.log("OK");
			return result;
		}
		/* Return HttpError if unsuccessful
		*/ 
		else {
			console.log("error"); 
			//throw new HttpError(response, "Could not retrieve items at CMS.")
		}
	}

	//import {getEmojiItems} from '$lib/loadHelper'
	// import HttpError from '$lib/HttpError'
	function load() {
		//let tokenResult;
		let pageItemsResult;
		try {
			// tokenResult = await getAccessToken(fetch)
			result = getEmojiItems(); //), tokenResult.data["access_token"])
			/* If successful, Pass props object to page. 
			*/
			return {
				props: {
					items: result,
					//tokenResult: tokenResult
				}
			}
		} catch (error) {
			/* If unsuccessful, pass an object containing error and status to page.
			*/
			return {
				/* Since Error does not contain the status, we use HttpError.
				*/
				// status: error instanceof HttpError ? error.response.status : 500,
				error: error
			}
		}
	}

	const promise = getEmojiItems();
 

	//export let pageItemsResult;

	//console.log(pageItemsResult);

	import { fade, fly } from 'svelte/transition';
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>
<main>
	{#await promise}
 		<p>Loading...</p>
	{:then items} 
		<h1 in:fly="{{ y: 200, duration: 2000 }}">{items.data[Math.floor(Math.random()*items.data.length)].Emoji}</h1> 
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}


 
	<div class="footer"> 
		<p> Solucion para Reto #1 de <a href="https://github.com/malandrinersdev/backend-de-la-suerte"> Backend de la suerte</a> | Visita la comunidad <a href="https://danielprimo.io">DanielPrimo.io</a> para otros retos en pantuflas como este | Done with ❤️ </p></div>
  
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
		font-size: .8em;
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