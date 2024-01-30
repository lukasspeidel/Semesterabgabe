<script>
    import { navigate } from 'svelte-routing';
    import {Link} from 'svelte-routing';
</script>

<main>
    <button on:click={() => navigate("/character")}>
        Go to Character Panel
    </button>
    <img id="background-image" src="https://cdn.mos.cms.futurecdn.net/8vM5KTsVVLgCCxFwWwYLfG.jpg" alt="hyperspace" />
</main>

<style>
    main {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
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
</style>

