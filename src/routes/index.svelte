<svelte:head>
    <title>Corona information</title>
</svelte:head>

<script>
    import {onMount} from "svelte"
    import Loader from "../components/Loader.svelte"
    import World from "../components/World.svelte"
    import {db} from "../firestore.js"

    let world = []
    let searched = []
    let AllOverWorld

    onMount(() => {
        fetch(`Corona.json`)
            .then(response => response.json())
            .then(json => {
                // World
                world = json.Countries
                searched = world
                AllOverWorld = json.Global
            })
            .catch((err) => console.log(err))
    })

    let order = "title"
    $ : switch (order) {
        case 'title':
            searched = world.sort((a, b) => a.Country > b.Country
                ? 1
                : -1);
            break
        case 'death':
            searched = world.sort((a, b) => a.TotalDeaths < b.TotalDeaths
                ? 1
                : -1);
            break
        case 'infected':
            searched = world.sort((a, b) => a.TotalConfirmed < b.TotalConfirmed
                ? 1
                : -1);
            break
        case 'recovered':
            searched = world.sort((a, b) => a.TotalRecovered < b.TotalRecovered
                ? 1
                : -1);
            break
    }

    let search = ""
    const filterResult = () => {
        if (search === " ") {
            searched = world
        } else {
            searched = world.filter(world => world.Country.toLowerCase().includes(search.toLowerCase()))
        }
    }
    const addToFavorite = (item) => {
        db
            .collection("favorites")
            .doc(item.Country)
            .set({name: item.Country, death: item.TotalDeaths, infected: item.TotalConfirmed, recovered: item.TotalRecovered})
    }

    const buttons = [
        {
            order: "Title",
            orderType: "title"
        }, {
            order: "Death",
            orderType: "death"
        }, {
            order: "Infected",
            orderType: "infected"
        }, {
            order: "Recovered",
            orderType: "recovered"
        }
    ]
</script>

<div>
    <label for="btn">Sort by:</label>
    <div class="sort" id="myDiv">
        {#each buttons as button}
            <button id="btn" class="button" on:click={() => order = `${button.orderType} `}>{button.order}</button>
        {/each}

    </div>
    <div class="searchbar">
        <input
            autocomplete="off"
            class="search-input"
            type="search"
            placeholder="Search after country"
            bind:value={search}
            on:input|preventDefault={filterResult}/>
    </div>
    <section>
        {#each searched as item}
            <World item={item} addToFavorite={addToFavorite}/>
        {:else}
            <Loader/>
        {/each}
    </section>

</div>

<style>

    div {
        display: grid;
        background-color: #1b1b30;
        width: 100vw;
        justify-content: center;
    }
    section {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        justify-content: center;
        gap: 2rem;
        margin: auto;
        width: 70vw;
        min-height: 100vh;
    }
    .sort {
        display: grid;
        grid-template-columns: 100px 100px 100px 100px;
        gap: 10px;
        width: 90vw;
        margin: auto;
        justify-content: center;
    }
    label {
        color: white;
        margin: auto;
        padding: 1rem;
        text-align: 1.9rem;
    }
    .button {
        height: 30px;
        background-color: indigo;
        color: white;
        outline: none;
    }
    .searchbar {
        align-content: center;
        width: 100vw;
        text-align: center;
    }
    .search-input {
        border-radius: 4px;
        border: 1px solid black;
        padding: 1rem;
        font-size: 20px;
        margin: 15px;
        outline: none;
        width: 30rem;
    }

</style>