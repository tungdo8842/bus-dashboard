<script>
    let { route_short_name, trip_headsign, route_color, time } = $props();

    // time from the api is in second, not ms, so * 1000 to convert to ms
    let wait_time = $derived(time * 1000 - Date.now());
</script>

<div class="trip-container">
    <div class="first-row">
        <div class="route-number">
            <p style="background-color: #{route_color};">{route_short_name}</p>
        </div>
        <div class="trip-info">
            <p>{trip_headsign}</p>
        </div>
        <div class="time-info">
            {#if wait_time <= (-60)*60*1000}
                <p></p>
            {:else if wait_time <= (-60)*1000}
                <p>{new Date(Math.abs(wait_time)).getMinutes()} min ago</p>
            {:else if wait_time <= 60*1000}
                <p>Now</p>
            {:else if wait_time/1000/60 >= 60}
                <p>{new Date(time*1000).getHours()}:{("0" + (new Date(time*1000).getMinutes())).slice(-2)}</p>
            {:else}
                <p>{new Date(wait_time).getMinutes()} min</p>
            {/if}
        </div>
    </div>
    <div class="second-row"></div>
</div>

<style>
    .trip-container {
        display: flex;
        flex-direction: column;
        border: 1px solid gray;
        border-radius: 8px;
        margin: 8px;
        max-width: 400px;
        padding: 0px 8px;
    }

    .first-row {
        display: flex;
        gap: 10px;
        flex-direction: row;
    }

    /* for bus number */
    .route-number p {
        height: 2.4em;
        width: 2.4em;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
        border-radius: 8px;
        font-family: sans-serif;
        font-weight: bold;
    }

    .trip-info {
        flex-grow: 2;
    }

    .time-info {
        text-align: right;
        width: 5rem;
        flex-grow: 0;
    }

    .time-info p {
        font-family: sans-serif;
    }
</style>
