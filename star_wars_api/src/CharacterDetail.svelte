<!-- CharacterDetail.svelte -->
<script>
    export let id;
  </script>
  
  <main>
    <h1>Character Detail</h1>
    <img src={`https://starwars-visualguide.com/assets/img/characters/${id}.jpg`} alt={`Character ${id}`} />
  </main>
  
  <style>
    main {
      text-align: center;
    }
  
    img {
      max-width: 100%;
      max-height: 80vh;
      object-fit: contain;
      margin-top: 20px;
    }
  </style>
  