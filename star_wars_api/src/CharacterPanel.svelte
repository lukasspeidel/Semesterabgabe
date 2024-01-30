<!-- People.svelte -->
<script>
  import { onMount, afterUpdate } from 'svelte';
  import Character from './components/Character.svelte';

  let people = [];
  let displayedPeople = [];
  let selectedGender = 'all'; // Default value, 'all' means no filter
  let startIndex = -9;
  const chunkSize = 9;

  async function fetchData() {
    try {
      let currentPage = 1;
      let totalPages = 1;

      while (currentPage <= totalPages) {
        const response = await fetch(`https://swapi.dev/api/people/?page=${currentPage}`);
        const data = await response.json();

        // Add an id property to each character in the data.results array
        const newCharacters = data.results.map((character, index) => ({
          ...character,
          id: (currentPage - 1) * data.results.length + index + 1,
        }));

        people = [...people, ...newCharacters];

        // Update totalPages based on the API response
        totalPages = Math.ceil(data.count / data.results.length);

        currentPage++;
      }

/*       // Fetch homeworld information for each person
      const homeworldsData = await Promise.all(people.map(person => fetchHomeworld(person)));

      // Update homeworldName property based on fetched data
      homeworldsData.forEach((homeworld, index) => {
        people[index].homeworldName = homeworld.name;
      }); */

      // Initial display of 10 people
      displayedPeople = people.slice(startIndex, startIndex + chunkSize);
    } catch (error) {
      console.error('Error fetching data:', error);
    }
    console.log(people);
  }

  onMount(() => {
    fetchData();
  });

  afterUpdate(() => {
    // Additional logic after the component updates (if needed)
  });

  function filterByGender(person) {
    if (selectedGender === 'all') {
      return true; // Show all characters when no filter is selected
    } else {
      return person.gender === selectedGender;
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

  function loadMorePeople() {
    startIndex += chunkSize;
    displayedPeople = people.slice(startIndex, startIndex + chunkSize);
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
    {#each displayedPeople.filter(filterByGender) as person (person.id)}
      <Character
        id={person.id}
        name={person.name}
        birth_year={person.birth_year}
        homeworld={person.homeworldName}
        imageurl={`https://starwars-visualguide.com/assets/img/characters/${person.id}.jpg`}
      />
    {/each}
  </div>

  <!-- Load more button -->
  {#if startIndex + chunkSize < people.length}
    <button on:click={loadMorePeople}>Load More</button>
  {/if}
</main>

<style>
  #character-panel-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 10px;
  }

  main {
    max-width: 800px;
    margin: 0 auto;
  }


</style>
