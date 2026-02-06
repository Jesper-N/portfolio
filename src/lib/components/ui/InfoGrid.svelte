<script lang="ts">
    import { onMount } from "svelte";
    import TactileCard from "./TactileCard.svelte";
    import TechStackCard from "./TechStackCard.svelte";

    type MapComponent = typeof import("$lib/components/ui/map/Map.svelte").default;
    type MapMarkerComponent = typeof import("$lib/components/ui/map/MapMarker.svelte").default;

    interface LoadedMapComponents {
        Map: MapComponent;
        MapMarker: MapMarkerComponent;
    }

    const MAP_CENTER: [number, number] = [9.4, 56.45];
    const MAP_ZOOM = 11;
    const MAP_STYLES = {
        dark: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
        light: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
    };
    const MAP_OPTIONS = {
        interactive: false,
        attributionControl: false as const,
    };

    let time = $state(new Date());
    const timeFormatter = new Intl.DateTimeFormat("en-DK", {
        hour: "2-digit",
        minute: "2-digit",
        second: "2-digit",
        timeZone: "Europe/Copenhagen",
    });
    const formattedTime = $derived(timeFormatter.format(time));

    let hovered = $state(false);
    let mapContainerRef: HTMLDivElement | null = $state(null);
    let mapComponentsPromise = $state<Promise<LoadedMapComponents> | null>(null);

    function loadMapComponents() {
        if (mapComponentsPromise) return;
        mapComponentsPromise = Promise.all([
            import("$lib/components/ui/map/Map.svelte"),
            import("$lib/components/ui/map/MapMarker.svelte"),
        ]).then(([mapModule, mapMarkerModule]) => ({
            Map: mapModule.default,
            MapMarker: mapMarkerModule.default,
        }));
    }

    onMount(() => {
        const interval = setInterval(() => {
            time = new Date();
        }, 1000);

        let observer: IntersectionObserver | null = null;
        if (typeof IntersectionObserver === "undefined" || !mapContainerRef) {
            loadMapComponents();
        } else {
            observer = new IntersectionObserver(
                (entries) => {
                    if (entries.some((entry) => entry.isIntersecting)) {
                        loadMapComponents();
                        observer?.disconnect();
                    }
                },
                { rootMargin: "200px 0px" },
            );
            observer.observe(mapContainerRef);
        }

        return () => {
            clearInterval(interval);
            observer?.disconnect();
        };
    });
</script>

<div class="grid grid-cols-1 md:grid-cols-2 gap-6">

    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <TactileCard
        class="min-h-75 flex flex-col justify-between relative overflow-hidden p-0"
        style="isolation: isolate;"
    >
        <!-- Map fills entire card, brightness controlled via JS for smooth 60fps -->
        <div
            bind:this={mapContainerRef}
            class="absolute inset-0 z-0 map-container"
            class:map-active={hovered}
            onmouseenter={() => hovered = true}
            onmouseleave={() => hovered = false}
            role="presentation"
        >
            {#if mapComponentsPromise}
                {#await mapComponentsPromise}
                    <div class="absolute inset-0 bg-[#0a0a0a]"></div>
                {:then components}
                    {@const Map = components.Map}
                    {@const MapMarker = components.MapMarker}
                    <Map
                        center={MAP_CENTER}
                        zoom={MAP_ZOOM}
                        styles={MAP_STYLES}
                        theme="dark"
                        options={MAP_OPTIONS}
                    >
                        <MapMarker longitude={MAP_CENTER[0]} latitude={MAP_CENTER[1]}>
                            <div class="h-3 w-3 rounded-full bg-green-500 shadow-[0_0_10px_rgba(34,197,94,0.8)] animate-pulse"></div>
                            <div class="absolute inset-0 h-3 w-3 rounded-full border border-green-500/50 animate-[ping_2s_linear_infinite]"></div>
                        </MapMarker>
                    </Map>
                {:catch}
                    <div class="absolute inset-0 bg-[#0a0a0a]"></div>
                {/await}
            {:else}
                <div class="absolute inset-0 bg-[#0a0a0a]"></div>
            {/if}
        </div>

        <!-- Text at top left -->
        <div class="relative z-10 p-8 pointer-events-none">
            <h3 class="font-mono text-sm uppercase tracking-widest text-muted-foreground mb-1">Located Near</h3>
            <h2 class="text-3xl font-medium text-white">Viborg, Denmark</h2>
            <p class="text-sm text-muted-foreground mt-2 font-mono">
                {formattedTime} (CET)
            </p>
        </div>

        <!-- Status at bottom with scrim -->
        <div class="relative z-10 mt-auto p-8 pt-12 map-text-scrim pointer-events-none">
            <div class="flex items-center gap-3 text-sm text-muted-foreground/80">
                <span class="relative flex h-2 w-2">
                  <span class="animate-ping absolute inline-flex h-2 w-2 rounded-full bg-green-400 opacity-75"></span>
                  <span class="relative inline-flex rounded-full h-2 w-2 bg-green-500"></span>
                </span>
                Open to Remote & Relocation
            </div>
        </div>
    </TactileCard>

    <TechStackCard />
</div>

<style>
    .map-container {
        opacity: 0.4;
        transition: opacity 0.35s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    }

    .map-container.map-active {
        opacity: 1;
    }

    .map-text-scrim {
        background: linear-gradient(
            to top,
            oklch(0.13 0 0) 0%,
            oklch(0.13 0 0 / 0.92) 20%,
            oklch(0.13 0 0 / 0.7) 45%,
            oklch(0.13 0 0 / 0.3) 70%,
            oklch(0.13 0 0 / 0) 100%
        );
    }
</style>
