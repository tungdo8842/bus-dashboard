<script>
    import { onMount } from "svelte";
    import TripDisplay from "./TripDisplay.svelte";

    let trips = $state();
    let trips_snapshot;

    onMount(async () => {
        await fetch(
            "http://localhost:8000/bus/uvic-departures",
            {
                method: "GET",
                headers: { "Content-Type": "application/json" },
            },
        ).then(response => response.json()).then(data => {trips = data;});
    });
    $inspect(trips);

</script>
{#each trips as trip}
    <TripDisplay route_short_name={trip.route_short_name} route_long_name={trip.route_long_name}
    route_color={trip.route_color} time={trip.time}/>
{/each}

