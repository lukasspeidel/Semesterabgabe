<!-- CharacterDetails.svelte -->
<script>
    import { onMount } from 'svelte';

    export let id;
    export let name;
    export let birth_year;
    export let homeworld;
    export let imageurl;

    if (id === 17) {
        id = 18;
    } else if (id > 17) {
        id = id + 1;
    }

    onMount(async () => {
        try {
            const response = await fetch(`https://swapi.dev/api/people/${id}/`);
            const data = await response.json();

            // Fetch homeworld data
            const homeworldResponse = await fetch(data.homeworld);
            const homeworldData = await homeworldResponse.json();

            // Extract homeworld name from the object
            homeworld = homeworldData.name || 'Unknown';
        } catch (error) {
            console.error(`Error fetching homeworld data for ${name}:`, error);
            homeworld = 'Unknown';
        }
    });
</script>

<main>
    <div class="character-panel">
        <!-- Display character image using the specified source -->
        <div class="character-image"><img src={`https://starwars-visualguide.com/assets/img/characters/${id}.jpg`} alt={name} /></div>

        <p>Name: {name}</p>
        <p>Birth Year: {birth_year}</p>
        <p>Home Planet: {homeworld}</p>

    </div>

</main>



<style>
    @import url('https://fonts.googleapis.com/css2?family=Jura:wght@300&display=swap');

    .character-panel {
        width: 240px;
        height: 470px; 
        font-family: 'Jura', sans-serif;
        font-size: 1.2em;
        background-color: #272727;
        border-radius: 1.2em;
        font-weight: bold;
        text-align: center;
        color: white;
    }

    img {
        width: 100%;
        height: 80%; 
        object-fit: cover;
        border-radius: 1.2em 1.2em 0 0;
    }

    p {
        margin: 0.5em;
    }
</style>
