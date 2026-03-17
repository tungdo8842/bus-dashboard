<script>
    let { route_short_name, route_long_name, route_color, time } = $props();

    // in seconds
    let wait_time = $derived(time * 1000 - Date.now());
</script>

<div class="trip-container">
    <div class="first-row">
        <div class="route-number">
            <p style="background-color: #{route_color};">{route_short_name}</p>
        </div>
        <div class="trip-info">
            <p>{route_long_name}</p>
        </div>
        <div class="time-info">
            {#if wait_time >= 0}
                <p>{new Date(wait_time).getMinutes()} min</p>
            {:else}
                <p>{new Date(Math.abs(wait_time)).getMinutes()} min ago</p>
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
        width: 4rem;
        flex-grow: 0;
    }
</style>
