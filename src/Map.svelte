<script>
	import { onDestroy, setContext } from 'svelte';
	import { maplibregl, key } from './maplibre.js';

	setContext(key, {
		getMap: () => map,
	});

	export let lat;
	export let lon;
	export let zoom;

	let container;
	let map;

	function load() {
		map = new maplibregl.Map({
			container,
			style: {
            "version": 8,
            "sources": {
                "raster-tiles": {
                    "type": "raster",
                    "tiles": [
                        "https://a.tile.openstreetmap.org/{z}/{x}/{y}.png"
                        // "https://a.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png"
                        // "http://mt0.google.com/vt/lyrs=m&x={x}&y={y}&z={z}"
                        // "https://mt0.google.com/vt/lyrs=m@169000000&hl=ru-RU&x=2492&y=1312&zoom=5"
                    ],
                    "tileSize": 256,
                    // "attribution": "&copy; <a href=\"https://www.openstreetmap.org/copyright\">OpenStreetMap</a> contributors"
                    "attribution": "&copy; Google"
                }
            },
            "layers": [{
                    "id": "simple-tiles",
                    "type": "raster",
                    "source": "raster-tiles",
                    "minzoom": 0,
                    "maxzoom": 22
                }
            ]
        }
,
			center: [lon, lat],
			zoom,
		});
	}

	onDestroy(() => {
		if (map) map.remove();
	});
</script>

<svelte:head>
	<link
		rel="stylesheet"
		href="https://unpkg.com/maplibre-gl@2.4.0/dist/maplibre-gl.css"
		on:load={load}
	/>
</svelte:head>

<div bind:this={container}>
	{#if map}
		<slot />
	{/if}
</div>

<style>
	div {
		width: 100vw;
		height: 100vh;
	}
</style>
