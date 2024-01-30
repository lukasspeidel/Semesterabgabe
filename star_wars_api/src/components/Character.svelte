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
        <p>ID: {id}</p>
        <p>Name: {name}</p>
        <p>Age: {birth_year}</p>
        <p>Home Planet: {homeworld}</p>
        <hr />
    </div>
</main>

<style>
    .character-panel {
        width: 200px;
        height: 600px; /* Adjusted height for better image display */
    }

    img {
        width: 100%;
        height: 80%; /* Adjusted height for better image display */
        object-fit: cover;
    }
</style>
