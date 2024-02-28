<!-- TODO: use specify types -->
<script>
    import { onMount } from "svelte";

    let tokens;
    let errorMessage;

    onMount(async () => {
        await fetchTokens();
    });

    async function fetchTokens() {
        try {
            const response = await fetch(
                "http://localhost:8000/api/v1/tokens",
                {
                    method: "GET",
                    headers: {
                        Accept: "application/json",
                    },
                },
            );

            if (!response.ok) {
                throw new Error("Failed to fetch tokens");
            }

            const res = await response.json();
            tokens = res.data;
            console.log(tokens);
        } catch (error) {
            errorMessage = error.message;
            console.log(errorMessage);
        }
    }
</script>

<div></div>
{#if !tokens}
    <div class="loader centered"></div>
{:else}
    {#each tokens as token}
        <div class="coin-card horizontal-flex">
            <img alt="" src={token.thumb} />
            <p>{token.token}</p>
        </div>
    {/each}
{/if}

<style>
    .centered {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translateX(-200%);
    }

    .loader {
        border: 9px solid rgba(0, 0, 0, 0.1);
        border-left-color: #fafafa; /* Adjust the color as needed */
        border-radius: 50%;
        width: 200px;
        height: 200px;
        animation: spin 1s linear infinite;
    }

    .horizontal-flex {
        display: flex;
        flex-direction: row;
    }

    .coin-card {
        background: #ffffff;
        border-radius: 16px;
    }

    @keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
    :global(html, body) {
        margin: 0;
        padding: 0;
        width: 100%;
        height: 100%;
    }
</style>
