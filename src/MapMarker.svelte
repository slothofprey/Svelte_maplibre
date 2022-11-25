<script>
	import { getContext } from 'svelte';
	import { maplibregl, key } from './maplibre.js';

	const { getMap } = getContext(key);
	const map = getMap();

	export let lat;
	export let lon;
	export let label;

	const popup = new maplibregl.Popup({ offset: 25 })
		.setText(label);

	const marker = new maplibregl.Marker()
		.setLngLat([lon, lat])
		.setPopup(popup)
		.addTo(map);

</script>