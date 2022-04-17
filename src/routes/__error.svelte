<script context="module">
    export function load({ error, status }) {
        return {
            props: {
                errorMessage: `${status}: ${error.message}`,
            },
        };
    }
</script>

<script>
    export let errorMessage;
</script>

<main>
    <h1>{errorMessage}</h1>
</main>
