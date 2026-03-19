<script>
    import { onMount } from "svelte";
    import TripDisplay from "./TripDisplay.svelte";

    let trips = $state();
    let trips_snapshot;

    async function updateTrip() {
        await fetch(
            "http://localhost:8000/bus/uvic-departures",
            // "https://api.tungdo.dev/bus/uvic-departures",
            {
                method: "GET",
                headers: { "Content-Type": "application/json" },
            },
        )
            .then((response) => response.json())
            .then((data) => {
                trips = data;
            });
    }

    onMount(async () => {
        await updateTrip();
        setInterval(updateTrip, 30000);
    });
    // $inspect(trips);
</script>

<svelte:document on:visibilitychange={ () => {
    if (document.visibilityState === "visible") { updateTrip(); }
}} />

<h3 class="uvic-header">Departures from UVic bus loop</h3>
{#each trips as trip}
    <TripDisplay
        route_short_name={trip.route_short_name}
        trip_headsign={trip.trip_headsign}
        route_color={trip.route_color}
        time={trip.time}
    />
{/each}
<p>Data Source: <a href="https://www.bctransit.com/open-data/">BC Transit</a></p>

<style>
    .uvic-header {
        font-family: sans-serif;
    }

    :global(body) {
        background-color: #ffffff;
        color: #000000;
        /* background-color: #1b1b1b; */
        /* color: #eeeeee; */
    }
</style>
