<!-- People.svelte -->
<script>
  import { onMount, afterUpdate } from 'svelte';
  import Character from '../components/Character.svelte';

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


<header>
  <h1 class="headerText">Star Wars Characters</h1>
<!-- Dropdown menu for selecting gender -->
<label for="genderFilter">Filter by Gender:</label>
<select id="genderFilter" bind:value={selectedGender}>
  <option value="all">All</option>
  <option value="male">Male</option>
  <option value="female">Female</option>
  <option value="hermaphrodite">Hermaphrodite</option>
  <!-- Add more options based on your dataset -->
</select>
  <!-- Load more button -->
  {#if startIndex + chunkSize < people.length}
    <button class="LoadMoreButton" on:click={loadMorePeople}>Load More</button>
  {/if}
</header>

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

  <img id="background-image" src="https://cdn.mos.cms.futurecdn.net/8vM5KTsVVLgCCxFwWwYLfG.jpg" alt="hyperspace" />
</main>

<style>


  #character-panel-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 10px;
    font-family: 'Jura', sans-serif;
  }

  main {
    padding-top: 120px; /* Add padding to the top of the main content to account for the fixed header */
    max-width: 800px;
    margin: 0 auto;
    font-family: 'Jura', sans-serif;
  }

  #genderFilter {
    margin-bottom: 1em;
  }

  .LoadMoreButton {
    background-color: #BE8900;
    color: white;
    height: 40px;
    width: 240px;
    border-radius: 5px;
    border: none;
    font-size: 1em;
    font-weight: bold;
    cursor: pointer;
    font-family: 'Jura', sans-serif;
    font-weight: 800;

  }

  #genderFilter {
    margin-top: 9px;
    background-color: #BE8900;
    color: white;
    height: 40px;
    width: 240px;
    border-radius: 5px;
    border: none;
    font-size: 1em;
    font-weight: bold;
    cursor: pointer;
    font-family: 'Jura', sans-serif;
    font-weight: 400;
  }

  header {
  padding-left: 10px;
  padding-right: 10px;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #272727;
  color: white;
  font-family: 'Jura', sans-serif;
  font-weight: 800;
  z-index: 1000;
}

.headerText {
  margin-bottom: 28px;
}

  
  #background-image {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1; /* Set a lower z-index to ensure the background is behind other elements */
    object-fit: cover; /* Cover the entire viewport */
  }

  header label,
header select {
  margin-bottom: 10px;
}

header .LoadMoreButton {
  margin-bottom: 9px;
}

@media only screen and (min-width: 800px) {
  header {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }

  header h1 {
    margin-bottom: 0;
  }

  header label,
  header select {
    margin: 0;
  }

  header .LoadMoreButton {
    margin-top: 0;
    margin-right: 0;
  }

  #character-panel-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-between;
  }

  .LoadMoreButton {
    margin-top: 0;
    margin-right: 0;
  }
}


</style>
