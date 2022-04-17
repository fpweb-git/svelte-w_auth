<script>
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    let name;
    let loader = false;
    let articles = [];

    onMount(async () => {
        if (localStorage.getItem("jwt")) {
            try {
                const res = await fetch(
                    "http://localhost:1337/api/articles?populate=*",
                    {
                        headers: {
                            Authorization:
                                "Bearer " + localStorage.getItem("jwt"),
                        },
                    }
                );
                if (!res.ok) {
                    try {
                        const data = await res.json();
                        const error = data.error;
                        throw {
                            status: error.status,
                            message: error.message,
                        };
                    } catch (err) {
                        console.log(err);
                        throw err;
                    }
                } else {
                    const json = await res.json();
                    console.log(json);
                    articles = json.data;
                    loader = true;
                }
            } catch (err) {
                goto("/login", { replaceState: true });
            }
        } else {
            goto("/login", { replaceState: true });
        }
        if (localStorage.getItem("user")) {
            name = localStorage.getItem("user");
        }
    });
</script>

{#if loader}
    <main>
        <h1>Profile page</h1>
        <h2>Welcome {name}</h2>
        {#each articles as article (article.id)}
            <article>
                <header><h2>{article.attributes.title}</h2></header>
                <body>
                    <p>
                        {article.attributes.content}
                    </p>
                </body>
            </article>
        {/each}
    </main>
{:else}
    <div aria-busy="true" class="loader" />
{/if}

<style>
    .loader {
        position: fixed;
        top: 0;
        left: 0;
        height: 100vh;
        width: 100%;
        z-index: 40;
        display: flex;
        justify-content: center;
        align-items: center;
        background: rgb(35, 43, 59);
        background: linear-gradient(
            148deg,
            rgba(35, 43, 59, 1) 0%,
            rgba(53, 53, 93, 1) 59%,
            rgb(64, 42, 105) 100%
        );
    }
</style>
