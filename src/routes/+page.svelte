<script>
    import { onMount } from "svelte";
    import BusStop from "./BusStop.svelte";
</script>

<BusStop endpoint="bus/uvic-departures" stopName="UVic Bus Loop" args="" />

<p>
    Data Source: <a
        href="https://www.bctransit.com/open-data/"
        style="color: white;">BC Transit</a
    >
</p>
