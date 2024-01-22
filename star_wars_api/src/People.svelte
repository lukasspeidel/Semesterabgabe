<script>
  import { onMount } from 'svelte';

  let people = [];

  async function fetchData() {
    try {
      let currentPage = 1;
      let totalPages = 1;

      while (currentPage <= totalPages) {
        const response = await fetch(`https://swapi.dev/api/people/?page=${currentPage}`);
        const data = await response.json();
        people = [...people, ...data.results];

        // Update totalPages based on the API response
        totalPages = Math.ceil(data.count / data.results.length);

        currentPage++;
      }

      // Fetch homeworld information for each person
      const homeworldsData = await Promise.all(people.map(person => fetchHomeworld(person)));

      // Update homeworldName property based on fetched data
      homeworldsData.forEach((homeworld, index) => {
        people[index].homeworldName = homeworld.name;
      });
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  }

  async function fetchHomeworld(person) {
    try {
      const response = await fetch(person.homeworld);

      if (!response.ok) {
        console.error(`Error fetching homeworld data for ${person.name}: ${response.statusText}`);
        return { name: 'Unknown' }; // Provide a default value if homeworld data is not available
      }

      const data = await response.json();
      return data;
    } catch (error) {
      console.error(`Error fetching homeworld data for ${person.name}:`, error);
      return { name: 'Unknown' }; // Provide a default value if homeworld data is not available
    }
  }

  onMount(() => {
    fetchData();
  });
</script>

<main>
  <div id="character-panel-container">
    {#each people as person (person.url)}
      <div class="character-panel">
        <p>Name: {person.name}</p>
        <p>Age: {person.birth_year}</p>
        {#if person.homeworldName}
          <p>Home Planet: {person.homeworldName}</p>
        {:else}
          <p>Home Planet: Unknown</p>
        {/if}
        <hr />
      </div>
    {/each}
  </div>
</main>

<style>
  #character-panel-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }

  main {
    max-width: 600px;
    margin: 0 auto;
  }

  .character-panel {
    width: 200px;
    height: 200px;
  }
</style>
