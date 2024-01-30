<script>
    import { navigate } from 'svelte-routing';
    import {Link} from 'svelte-routing';
</script>

<main>
    <button on:click={() => navigate("/character")}>
        Go to Character Panel
    </button>
</main>

<style>
    main {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }
</style>

