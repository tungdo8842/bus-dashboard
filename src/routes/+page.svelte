<script>
    import { onMount } from "svelte";
    import { env } from "$env/dynamic/public";
    import BusStop from "./BusStop.svelte";
    let stop_list = $state();
    let added_stops = $state([]);
    let stop_select_box;

    onMount(async () =>{
        await fetch(
            env.PUBLIC_API_HOST + "/bus/vic/all_stops",
            {
                method: "GET",
                headers: { "Content-Type": "application/json" },
            },
        )
            .then((response) => response.json())
            .then((data) => {
                stop_list = data;
            });
    })

    // change to object/hashmap later
    function add_stop() {
        let stop_id = stop_select_box.value;
        let stop_name = stop_select_box.selectedOptions[0].text;
        added_stops.push([stop_id, stop_name]);
    }
</script>

<div class="stop-selection">
    <label for="stop-select">Stop: </label>
    <select id="stop-select" bind:this={stop_select_box}>
        {#each stop_list as stop}
            <option value="{stop[0]}">{stop[1]}</option>
        {/each}
    </select>
    <button onclick={add_stop}>Add Stop</button>
</div>

<div class="stops-list">
    <BusStop endpoint="/bus/uvic-departures" stopName="UVic Bus Loop" />
    <BusStop
        endpoint="/bus/vic/stops?stop_id="
        stopName="Fort St at Quadra St"
        args="100064"
    />
    {#each added_stops as stop}
    <BusStop
        endpoint="/bus/vic/stops?stop_id="
        stopName={stop[1]}
        args={stop[0]}
    />
    {/each}
</div>

<p class="data-source">
    Data Source: <a
        href="https://www.bctransit.com/open-data/"
        style="color: white;">BC Transit</a
    >
</p>

<style>
    .stop-selection {
        display: block;
        text-align: center;
        margin: 1rem;
    }

    .stops-list {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
    }

    .data-source {
        text-align: center;
    }
</style>
