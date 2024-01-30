<script>
  import {Router, Route} from 'svelte-routing';
   import CharacterPanel from './CharacterPanel.svelte';
   import home from './routes/home.svelte';
</script>

<main>
  <CharacterPanel />
  <Router>
    <Route path="/" component={home} />
  </Router>
</main>

<style>

</style>
