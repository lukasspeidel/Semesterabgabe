<!-- Button.svelte -->
<script>
    export let color = 'blue';
    export let height = 40;
    export let width = 250;

    // Declare a custom event to handle click
    export let clickHandler = () => {};
</script>

<button
    class="button"
    style="background-color: {color}; height: {height}px; width: {width}px"
    on:click={clickHandler}
>
    <p>Load More</p>
</button> 

<style>
    .button {
        background-color: var(--button-color, blue);
        color: white;
        border-radius: 5px;
        border: none;
        font-size: 1em;
        font-weight: bold;
        cursor: pointer;
    }
</style>
