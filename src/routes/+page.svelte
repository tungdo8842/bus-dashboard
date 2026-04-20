<script>
    import { onMount } from "svelte";
    import { env } from "$env/dynamic/public";
    import BusStop from "./BusStop.svelte";
    import SearchBox from "./SearchBox.svelte";

    let stopList = $state();
    let addedStops = $state({});
    let searchIsOpened = $state(false);
    let searchQuery = $state("");
    let searchResults = $state([]);
    let searchButton = $state();
    let stopListLoaded = $state(false);

    onMount(async () => {
        // JSON.parse expects strings
        addedStops = JSON.parse(localStorage.getItem("addedStops") ?? "{}");

        // fetch stop list from api
        await fetch(env.PUBLIC_API_HOST + "/bus/vic/all_stops", {
            method: "GET",
            headers: { "Content-Type": "application/json" },
        })
            .then((response) => response.json())
            .then((data) => {
                stopList = data;
                stopListLoaded = true;
            });
    });

    $effect(() => {
        localStorage.setItem("addedStops", JSON.stringify(addedStops));
    });

    function toggleSearch() {
        searchIsOpened = true;
        searchButton.style.visibility = "hidden";
    }

    function closeSearch() {
        searchIsOpened = false;
        searchButton.style.visibility = "visible";
    }
</script>

{#if searchIsOpened}
    <SearchBox
        bind:searchQuery
        bind:searchResults
        bind:addedStops
        {stopList}
        {closeSearch}
    />
{/if}

<div class="stops-list">
    <BusStop
        endpoint="/bus/uvic-departures"
        stopName="UVic Bus Loop"
        closable={false}
    />
    <!-- <BusStop -->
    <!--     endpoint="/bus/vic/stops?stop_id=" -->
    <!--     stopName="Fort St at Quadra St" -->
    <!--     args="100064" -->
    <!-- /> -->
    {#each Object.entries(addedStops) as [key, value]}
        <BusStop
            endpoint="/bus/vic/stops?stop_id="
            stopName={value}
            args={key}
            stopClose={() => {
                delete addedStops[key];
            }}
        />
    {/each}
</div>

{#if stopListLoaded}
    <div class="stop-search" bind:this={searchButton}>
        <button onclick={toggleSearch}>Add new stop</button>
    </div>
{/if}

<p class="data-source">
    Data Source: <a
        href="https://www.bctransit.com/open-data/"
        style="color: white;">BC Transit</a
    >
</p>

<style>
    .stop-search {
        text-align: center;
        flex-grow: 1;
    }

    .stops-list {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        flex-grow: 1;
    }

    .stop-search button {
        color: #cccccc;
        background-color: #111111;
        border-radius: 8px;
        padding: 0.5rem;
        margin: 1rem;
    }

    .data-source {
        text-align: center;
    }
</style>
