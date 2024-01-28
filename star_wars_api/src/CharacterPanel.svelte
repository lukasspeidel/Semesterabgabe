<script>
  import { onMount } from 'svelte';

  let people = [];
  let selectedGender = 'all'; // Default value, 'all' means no filter

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

  function filterByGender(person) {
    if (selectedGender === 'all') {
      return true; // Show all characters when no filter is selected
    } else {
      return person.gender === selectedGender;
    }
  }
</script>

<!-- Dropdown menu for selecting gender -->
<label for="genderFilter">Filter by Gender:</label>
<select id="genderFilter" bind:value={selectedGender}>
  <option value="all">All</option>
  <option value="male">Male</option>
  <option value="female">Female</option>
  <option value="n/a">N/A</option>
  <option value="hermaphrodite">Hermaphrodite</option>
  <!-- Add more options based on your dataset -->
</select>

<main>
  <div id="character-panel-container">
    {#each people.filter(filterByGender) as person (person.name)}
      <div class="character-panel">
<!--         <img src="https://starwars-visualguide.com/assets/img/characters/1.jpg" alt="{person.name}" /> -->
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