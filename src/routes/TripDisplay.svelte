<script>
    let { route_short_name, trip_headsign, route_color, time } = $props();

    // time from the api is in second, not ms, so * 1000 to convert to ms
    let waitTime = $derived(time * 1000 - Date.now());
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
            <div class="time-until-stop">
                {#if waitTime <= -60 * 60 * 1000}
                    <p></p>
                {:else if waitTime <= -60 * 1000}
                    <p>{new Date(Math.abs(waitTime)).getMinutes()} min ago</p>
                {:else if waitTime <= 60 * 1000}
                    <p>Now</p>
                {:else if waitTime < 60 * 60 * 1000}
                    <p>{new Date(waitTime).getMinutes()} min</p>
                {/if}
            </div>
            <div class="actual-bus-time">
                {#if new Date(time * 1000).getHours() > 12}
                    <p>
                        {new Date(time * 1000).getHours() - 12}:{(
                            "0" + new Date(time * 1000).getMinutes()
                        ).slice(-2) + " PM"}
                    </p>
                {:else}
                    <p>
                        {new Date(time * 1000).getHours()}:{(
                            "0" + new Date(time * 1000).getMinutes()
                        ).slice(-2) + " AM"}
                    </p>
                {/if}
            </div>
        </div>
    </div>
</div>

<style>
    .trip-container {
        display: flex;
        flex-direction: column;
        /* border-radius: 16px; */
        border-bottom: 1px solid #444444;
        width: min(400px, 75vw);
        margin: 0px 16px;
        /* background-color: #1a1a1a; */
        color: #cccccc;
    }

    .first-row {
        display: flex;
        flex-direction: row;
        align-items: center;
        gap: 10px;
    }

    .route-number p {
        font-size: 0.8rem;
        height: 1.6rem;
        width: 2rem;
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
        font-size: 0.8rem;
        display: flex;
        align-items: center;
    }

    .time-info {
        text-align: right;
        width: 5rem;
        flex-grow: 0;
        display: flex;
        flex-direction: column;
        align-items: right;
        justify-content: right;
    }

    .time-info p {
        font-size: 0.8rem;
        font-family: sans-serif;
        margin: 0;
        padding: 0;
    }

    .time-until-stop {
        margin-top: 0.2rem
    }
    
    .actual-bus-time {
        margin-bottom: 0.2rem
    }
</style>
