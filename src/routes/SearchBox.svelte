<script>
    import { onMount } from "svelte";
    import { stopPropagation } from "svelte/legacy";

    let { searchQuery, searchResults, stopList, closeSearch, addedStops } =
        $props();
    let searchInput;

    function handleSearchKey() {
        let query = searchQuery.toLowerCase();
        searchResults = stopList.filter((item) => {
            let searchItem = item[1].toLowerCase();
            let matched = 0,
                queryIndex = 0,
                itemIndex = 0;
            // increment every character match from left to right
            while (itemIndex < searchItem.length && queryIndex < query.length) {
                if (query[queryIndex] === searchItem[itemIndex]) {
                    matched += 1;
                    queryIndex += 1;
                }
                itemIndex += 1;
            }
            return matched == query.length;
        });
    }

    function addStop(e) {
        let parentElem = e.target.parentElement;
        let stopId = parentElem.id.toString();
        let stopName = parentElem.children[0].innerText;
        addedStops[stopId] = stopName;
    }

    onMount(() => {
        searchInput.focus();
    });
</script>

<div
    class="search-overlay"
    role="button"
    tabindex="-1"
    onclick={closeSearch}
    onkeydown={(e) => {
        if (e.key === "Escape") {
            closeSearch();
        }
    }}
>
    <div
        class="search-content"
        role="searchbox"
        tabindex="-2"
        onclick={(e) => e.stopPropagation()}
        onkeydown={(e) => e.stopPropagation()}
    >
        <div
            class="search-box"
            role="searchbox"
            tabindex="-1"
            onkeyup={handleSearchKey}
        >
            <input
                type="text"
                class="search-input"
                placeholder="Search..."
                bind:value={searchQuery}
                bind:this={searchInput}
            />
        </div>

        <div class="search-results">
            {#each searchResults as item}
                <div class="individual-result" id={item[0]}>
                    <p>{item[1]}</p>
                    <button onclick={addStop}>Add</button>
                </div>
            {/each}
        </div>
    </div>
</div>

<style>
    .search-overlay {
        position: fixed;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        width: 100vw;
        height: 100vh;
        /* top: 0px; */
        /* left: 0px; */
    }

    .search-content {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        flex-grow: 1;

        background-color: #111111;
        color: #cccccc;
        border: 2px solid #2a2a2a;
        border-radius: 16px;

        max-width: 95vw;
        max-height: 400px;

        padding: 1rem;
        box-sizing: border-box;
    }

    .search-box {
        flex-grow: 0;
    }

    .search-results {
        flex-grow: 1;

        width: 350px;
        max-width: 85vw;
        overflow-y: auto;
        margin: 1rem;
    }

    .individual-result {
        display: flex;
        flex-direction: row;
        align-items: center;
        border-bottom: 1px solid #444444;

        margin: 0;
        padding: 0 8px;
        box-sizing: border-box;
    }

    .individual-result p {
        flex-grow: 1;
    }

    @media (max-width: 600px) {
        .search-input{
            font-size: 16px;
        }
    }
</style>
