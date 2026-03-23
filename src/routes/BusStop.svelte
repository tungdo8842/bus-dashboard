<script>
    let { endpoint, stopName, args = "" } = $props();
    import { env } from "$env/dynamic/public";
    import { onMount } from "svelte";
    import TripDisplay from "./TripDisplay.svelte";

    let trips = $state();
    let trips_snapshot;

    async function updateTrip() {
        await fetch(
            env.PUBLIC_API_HOST + endpoint + args,
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
    <div class="stop-name">
        <h3>{stopName}</h3>
    </div>
    <div class="trip-list">
        {#each trips as trip}
            <TripDisplay
                route_short_name={trip.route_short_name}
                trip_headsign={trip.trip_headsign}
                route_color={trip.route_color}
                time={trip.time}
            />
        {/each}
    </div>
</div>

<style>
    .stop-container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        margin: 1rem 0.5rem;
        border: 2px solid #2a2a2a;
        border-radius: 16px;
        width: 450px;
        max-width: 100vw;
        background-color: #111111;
        color: #cccccc;
    }

    .stop-name {
        padding: 0px 2rem;
        flex-grow: 1;
    }

    .trip-list {
        height: 320px;
        overflow-y: auto;
    }
</style>
