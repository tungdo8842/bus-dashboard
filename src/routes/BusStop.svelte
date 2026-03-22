<script>
    let { endpoint, stopName, args } = $props();
    import { onMount } from "svelte";
    import TripDisplay from "./TripDisplay.svelte";

    let trips = $state();
    let trips_snapshot;

    async function updateTrip() {
        await fetch(
            "http://localhost:8000/" + endpoint + args,
            // "https://api.tungdo.dev/" + endpoint + args,
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
</script>

<svelte:document
    on:visibilitychange={() => {
        if (document.visibilityState === "visible") {
            updateTrip();
        }
    }}
/>

<div class="stop-container">
    <div>
        <h3>{stopName}</h3>
    </div>
    {#each trips as trip}
        <TripDisplay
            route_short_name={trip.route_short_name}
            trip_headsign={trip.trip_headsign}
            route_color={trip.route_color}
            time={trip.time}
        />
    {/each}
</div>

<style>
    :global(body) {
        background-color: #ffffff;
        color: #000000;
        /* background-color: #1b1b1b; */
        /* color: #eeeeee; */
    }

    .stop-container {
        display: flex;
        flex-direction: column;
    }
</style>
