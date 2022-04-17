<script>
    import { onMount } from "svelte";
    let name;

    // TODO : error handeling + redirect
    onMount(async () => {
        const res = await fetch("http://localhost:1337/api/users/me", {
            headers: {
                Authorization: "Bearer " + localStorage.getItem("jwt"),
            },
        });
        const json = await res.json();
        name = json.username;
        console.log(json);
    });
</script>

<main>
    <h1>Profile page</h1>
    <h2>Welcome {name}</h2>
</main>
