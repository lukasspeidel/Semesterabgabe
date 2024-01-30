<script>
  import {Router, Route} from 'svelte-routing';
   import CharacterPanel from './routes/CharacterPanel.svelte';
   import home from './routes/home.svelte';
</script>

<main>
  <!-- <CharacterPanel /> -->
  <Router>
    <Route path="/" component={home} />
    <Route path="/character" component={CharacterPanel} />
  </Router>
</main>

<style>

</style>
