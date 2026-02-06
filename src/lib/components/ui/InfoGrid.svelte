<script lang="ts">
    import { onMount } from "svelte";
    import TactileCard from "./TactileCard.svelte";
    import Map from "$lib/components/ui/map/Map.svelte";
    import MapMarker from "$lib/components/ui/map/MapMarker.svelte";
    import TechStackCard from "./TechStackCard.svelte";

    let time = $state(new Date());
    const timeFormatter = new Intl.DateTimeFormat("en-DK", {
        hour: "2-digit",
        minute: "2-digit",
        second: "2-digit",
        timeZone: "Europe/Copenhagen",
    });
    const formattedTime = $derived(timeFormatter.format(time));

    onMount(() => {
        const interval = setInterval(() => {
            time = new Date();
        }, 1000);
        return () => clearInterval(interval);
    });
</script>

<div class="grid grid-cols-1 md:grid-cols-2 gap-6">

    <TactileCard class="min-h-75 flex flex-col justify-between relative overflow-hidden group p-0">

        <div class="absolute inset-0 z-0 opacity-50 transition-opacity duration-700 group-hover:opacity-100 grayscale group-hover:grayscale-0">
             <Map
                center={[9.40, 56.45]}
                zoom={11}
                styles={{
                    dark: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
                    light: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json"
                }}
                theme="dark"
                options={{
                    interactive: false,
                    attributionControl: false
                }}
            >
                <MapMarker longitude={9.40} latitude={56.45}>
                    <div class="h-3 w-3 rounded-full bg-green-500 shadow-[0_0_10px_rgba(34,197,94,0.8)] animate-pulse"></div>
                    <div class="absolute inset-0 h-3 w-3 rounded-full border border-green-500/50 animate-[ping_2s_linear_infinite]"></div>
                </MapMarker>
            </Map>
        </div>


        <div class="absolute inset-0 z-10 bg-linear-to-t from-[#151515] via-[#151515]/60 to-transparent pointer-events-none"></div>


        <div class="relative z-20 p-8 h-full flex flex-col justify-between pointer-events-none">
            <div>
                <h3 class="font-mono text-sm uppercase tracking-widest text-muted-foreground mb-1">Located Near</h3>
                <h2 class="text-3xl font-medium text-white">Viborg, Denmark</h2>
                <p class="text-sm text-muted-foreground mt-2 font-mono">
                    {formattedTime} (CET)
                </p>
            </div>

            <div class="flex items-center gap-3 text-sm text-muted-foreground/80">
                <span class="flex h-2 w-2">
                  <span class="animate-ping absolute inline-flex h-2 w-2 rounded-full bg-green-400 opacity-75"></span>
                  <span class="relative inline-flex rounded-full h-2 w-2 bg-green-500"></span>
                </span>
                Open to Remote & Relocation
            </div>
        </div>
    </TactileCard>


    <TechStackCard />
</div>
