<script>
    import { navigate } from 'svelte-routing';
    import { Link } from 'svelte-routing';
</script>

<main>
    <Link to="/character">
        <button id="character-panel-button" on:click={() => navigate("/character")}>
            Go to Character Panel
        </button>
    </Link>
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
        z-index: -1;
        object-fit: cover;
    }

    #character-panel-button {
        background-color: #BE8900;
        color: white;
        height: 80px;
        width: 500px;
        border-radius: 5px;
        border: none;
        font-size: 2em;
        font-weight: bold;
        cursor: pointer;
        transition: box-shadow 0.3s; /* Added transition for smoother box-shadow change on hover */
    }

    #character-panel-button:hover {
        box-shadow: 0 0 20px rgba(190, 137, 0, 0.8); /* Added box-shadow effect on hover */
    }
</style>
