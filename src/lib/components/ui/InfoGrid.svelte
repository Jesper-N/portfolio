<script lang="ts">
    import { onMount } from "svelte";
    import TactileCard from "./TactileCard.svelte";
    import Map from "$lib/components/ui/map/Map.svelte";
    import MapMarker from "$lib/components/ui/map/MapMarker.svelte";

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

    const skills = [
        { category: "Core", items: ["Svelte", "SvelteKit", "TypeScript", "Tailwind CSS"] },
        { category: "Backend", items: ["Node.js", "Express", "PHP", "PostgreSQL"] },
        { category: "Systems", items: ["Embedded C", "Rust", "Linux", "Docker"] },
        { category: "AI/Data", items: ["OpenAI API", "Claude API", "TensorFlow", "Python"] }
    ];
</script>

<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
    
    <TactileCard class="min-h-[300px] flex flex-col justify-between relative overflow-hidden group p-0">
        
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
        
        
        <div class="absolute inset-0 z-10 bg-gradient-to-t from-[#151515] via-[#151515]/60 to-transparent pointer-events-none"></div>

        
        <div class="relative z-20 p-8 h-full flex flex-col justify-between pointer-events-none">
            <div>
                <h3 class="font-mono text-sm uppercase tracking-widest text-muted-foreground mb-1">Base of Operations</h3>
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

    
    <TactileCard class="p-8 min-h-[300px] flex flex-col relative overflow-hidden">
        <div class="absolute inset-0 bg-black/50"></div>
        <div class="relative z-10 flex items-center justify-between mb-6 border-b border-white/10 pb-4">
             <h3 class="font-mono text-sm uppercase tracking-widest text-muted-foreground">Tech_Stack.json</h3>
             <div class="flex gap-1.5">
                 <div class="h-2 w-2 rounded-full bg-red-500/20"></div>
                 <div class="h-2 w-2 rounded-full bg-yellow-500/20"></div>
                 <div class="h-2 w-2 rounded-full bg-green-500/20"></div>
             </div>
        </div>

        <div class="relative z-10 grid grid-cols-2 gap-y-6 gap-x-4">
            {#each skills as group}
                <div class="space-y-2">
                    <span class="text-xs font-mono text-white/40 block border-l border-white/10 pl-2">{group.category}</span>
                    <ul class="space-y-1">
                        {#each group.items as item}
                            <li class="text-sm text-muted-foreground hover:text-white transition-colors cursor-default flex items-center gap-2">
                                <span class="h-px w-2 bg-white/10"></span>
                                {item}
                            </li>
                        {/each}
                    </ul>
                </div>
            {/each}
        </div>
    </TactileCard>
</div>
