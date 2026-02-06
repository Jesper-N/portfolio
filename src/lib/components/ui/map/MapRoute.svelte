<script lang="ts">
	import { getContext } from "svelte";
	import MapLibreGL from "maplibre-gl";

	interface Props {
		
		id?: string;
		
		coordinates: [number, number][];
		
		color?: string;
		
		width?: number;
		
		opacity?: number;
		
		dashArray?: [number, number];
		
		onclick?: () => void;
		
		onmouseenter?: () => void;
		
		onmouseleave?: () => void;
		
		interactive?: boolean;
	}

	let {
		coordinates,
		color = "#4285F4",
		width = 3,
		opacity = 0.8,
		dashArray,
		onclick,
		onmouseenter,
		onmouseleave,
		interactive = true,
		id = crypto.randomUUID(),
	}: Props = $props();

	const mapCtx = getContext<{
		getMap: () => MapLibreGL.Map | null;
		isLoaded: () => boolean;
	}>("map");

	const sourceId = $derived(`route-source-${id}`);
	const layerId = $derived(`route-layer-${id}`);
	$effect(() => {
		const map = mapCtx.getMap();
		const loaded = mapCtx.isLoaded();

		if (!loaded || !map || coordinates.length < 2) return;
		if (map.getLayer(layerId)) map.removeLayer(layerId);
		if (map.getSource(sourceId)) map.removeSource(sourceId);
		map.addSource(sourceId, {
			type: "geojson",
			data: {
				type: "Feature",
				properties: {},
				geometry: {
					type: "LineString",
					coordinates,
				},
			},
		});
		const paint: any = {
			"line-color": "#94a3b8",
			"line-width": 5,
			"line-opacity": 0.6,
			"line-color-transition": { duration: 300, delay: 0 },
			"line-width-transition": { duration: 300, delay: 0 },
			"line-opacity-transition": { duration: 300, delay: 0 },
		};

		if (dashArray) {
			paint["line-dasharray"] = dashArray;
		}
		map.addLayer({
			id: layerId,
			type: "line",
			source: sourceId,
			layout: {
				"line-join": "round",
				"line-cap": "round",
			},
			paint,
		});

		return () => {
			try {
				if (map.getLayer(layerId)) map.removeLayer(layerId);
				if (map.getSource(sourceId)) map.removeSource(sourceId);
			} catch {
			}
		};
	});
	$effect(() => {
		const map = mapCtx.getMap();
		const loaded = mapCtx.isLoaded();

		if (!loaded || !map || coordinates.length < 2) return;

		const source = map.getSource(sourceId) as MapLibreGL.GeoJSONSource | undefined;
		if (source) {
			source.setData({
				type: "Feature",
				properties: {},
				geometry: {
					type: "LineString",
					coordinates,
				},
			});
		}
	});
	$effect(() => {
		const map = mapCtx.getMap();
		const loaded = mapCtx.isLoaded();

		if (!loaded || !map || !map.getLayer(layerId)) return;

		map.setPaintProperty(layerId, "line-color", color);
		map.setPaintProperty(layerId, "line-width", width);
		map.setPaintProperty(layerId, "line-opacity", opacity);

		if (dashArray) {
			map.setPaintProperty(layerId, "line-dasharray", dashArray);
		}
		if (opacity === 1) {
			try {
				map.moveLayer(layerId);
			} catch {
			}
		}
	});
	$effect(() => {
		const map = mapCtx.getMap();
		const loaded = mapCtx.isLoaded();

		if (!loaded || !map || !interactive) return;

		const handleClick = () => {
			onclick?.();
		};
		const handleMouseEnter = () => {
			map.getCanvas().style.cursor = "pointer";
			onmouseenter?.();
		};
		const handleMouseLeave = () => {
			map.getCanvas().style.cursor = "";
			onmouseleave?.();
		};

		map.on("click", layerId, handleClick);
		map.on("mouseenter", layerId, handleMouseEnter);
		map.on("mouseleave", layerId, handleMouseLeave);

		return () => {
			map.off("click", layerId, handleClick);
			map.off("mouseenter", layerId, handleMouseEnter);
			map.off("mouseleave", layerId, handleMouseLeave);
		};
	});
</script>
