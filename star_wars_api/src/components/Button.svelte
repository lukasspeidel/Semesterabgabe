<script>
    export let color = 'blue';
    export let height = 40;
    export let width = 250;
</script>

<style>
    .button {
        background-color: var(--button-color, blue);
        color: white;
        height: var(--button-height, 40px);
        width: var(--button-width, 250px);
        border-radius: 5px;
        border: none;
        font-size: 1em;
        font-weight: bold;
        cursor: pointer;
    }
</style>