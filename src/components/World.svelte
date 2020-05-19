<script>
	export let item
	export let addToFavorite
	import { db } from "../firestore.js"

	const favorites = db.collection("favorotes")

	    let favoritesCountry = []

    
        favorites.onSnapshot(snap => {
        favoritesCountry = snap.docs
	})
	
	const deleteFavorite = () => {
    
    	db.collection("favorites").doc(favoritesCountry.name).delete().then(function() {
    	    console.log("Document successfully deleted!");
    	}).catch(function(error) {
    	    console.error("Error removing document: ", error);
    	});
	}

	const found = favoritesCountry.find(fav => fav.name === item.Country)

</script>


<article >
	<h1 class="white">{item.Country}</h1>
	<p class="text orange">Total infected: {item.TotalConfirmed.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</p>
	<p class="text red">Total deaths: {item.TotalDeaths.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</p>
	<p class="text green">Total recovered: {item.TotalRecovered.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</p>
	{#if found}
		<button on:click={deleteFavorite(item)} class="btn">Remove from favorite</button>
	{:else}
		<button on:click={addToFavorite(item)} class="btn">Add to favorite</button>
	{/if}


</article>		

<style>
	article {
		background-color: rgba(4, 4, 4, .6);
		padding: 1rem;
		border-radius: 10px;
		display: grid;
		max-width: 15rem;
		max-height: 20rem;
	}
	article:hover {
		background-color: rgba(4, 4, 4, .2);
	}
	.btn {
		border: 1px solid white;
		background-color: indigo;
		outline: none;
		color: white;
		width: 50%;
		margin: auto;
		height: 2rem;;
		border-radius: 30px;
		box-shadow: 20px white;
	}
	.white {
		color: white;
		text-align: center;
		font-size: 1.8rem;
	}
	.green {
		color: green;
	}
	.red {
		color: red;
	}
	.orange {
		color: orange;
	}
</style>