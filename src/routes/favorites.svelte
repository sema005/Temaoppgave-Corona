<script>
	import { onMount } from "svelte"
    import { db } from "../firestore.js"
    import Favorite from "../components/Favorite.svelte"

    const favorites = db.collection("favorites")

    let favoritesCountry = []


        favorites.onSnapshot(snap => {
        favoritesCountry = snap.docs
    })


</script>

    <div class="back">
       <h1 class="white ">Favorites</h1>
        <section>
            <div>Name</div>
            <div class="orange">Total infected</div>
            <div class="red">Total deaths</div>
            <div class="green">Total recovered</div>
            <div></div>
        </section>
        {#each favoritesCountry as favorite}
            <Favorite favoritesCountry={favorite.data()}/>
        {:else}
            <p>U dont have any favorties yet</p>
        {/each}
    </div>


<style>
    .back {
        min-width: 100vw;
        min-height: 100vh;
        background-color: #1b1b30;
    }
    .white {
        color: white;
        text-align: center;
    }
    section{
        display: grid;
        grid-template-columns: 2fr 1fr 1fr 1fr 0.5fr;
        gap: 1rem;
        padding: 1rem;
        align-items: center;
        border-bottom: 1px solid #ccc;
        color: white;
        width: 80vw;
        margin: auto;
    }
    p {
        text-align: center;
        color: white;
    }
</style>