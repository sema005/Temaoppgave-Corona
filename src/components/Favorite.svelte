<script>
    import { db } from "../firestore.js"
    export let favoritesCountry




    const deleteFavorite = () => {
    
    db.collection("favorites").doc(favoritesCountry.name).delete().then(function() {
        console.log("Document successfully deleted!");
    }).catch(function(error) {
        console.error("Error removing document: ", error);
    });
}

</script>
    <section>
        <div>{favoritesCountry.name}</div>
        <div class="orange">{favoritesCountry.infected.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</div>
        <div class="red">{favoritesCountry.death.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</div>
        <div class="green">{favoritesCountry.recovered.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</div>
        <button on:click={deleteFavorite}>Remove</button>
    </section>


<style>
    section {
        display: grid;
        grid-template-columns: 2fr 1fr 1fr 1fr .5fr;
        gap: 1rem;
        padding: 1rem;
        align-items: center;
        border-bottom: 1px solid #ccc;
        color: white;
        width: 80vw;
        margin: auto;
    }

    button {
        background-color: indigo;
        color: white;
        outline: none;
    }
    .orange {
        color: orange
    }
    .red {
        color: red;
    }
    .green {
        color: green;
    }
</style>