<script lang="ts">
    import TactileCard from './TactileCard.svelte';
    import CardSpotlight from "./CardSpotlight.svelte";

    let { mouseX = null, mouseY = null, spotlightColor = "rgba(249, 115, 22, 0.06)" } = $props();
</script>

<TactileCard class="p-8 h-full relative group bg-[#0a0a0a] overflow-visible">
    <div class="absolute inset-0 z-0">
        <CardSpotlight color={spotlightColor} {mouseX} {mouseY} />
    </div>

    <div class="relative z-10 flex flex-col h-full pointer-events-none">
        <div class="flex items-center gap-2 mb-6">
            <span class="h-2 w-2 rounded-full bg-orange-500"></span>
            <span class="font-mono text-xs uppercase tracking-widest text-muted-foreground">Sim Racing</span>
        </div>

        <h3 class="text-2xl font-medium text-white mb-2">The Pursuit of Pace</h3>
         <p class="text-xs font-mono text-muted-foreground uppercase tracking-widest mb-6 border-b border-white/5 pb-6">
            Rookie Season (2025)
        </p>

        <div class="space-y-4 flex-1 pointer-events-auto">
            <p class="text-sm text-muted-foreground leading-relaxed">
                Recently acquired a full sim rig to translate my competitive FPS mindset into motorsport. Studying telemetry and physics to shave tenths off every lap.
            </p>

            <div class="pt-4 space-y-3">
                <span class="text-xs font-mono text-white/40 uppercase tracking-widest">Future Competition Targets</span>
                
                <div class="flex items-center gap-3 text-sm text-muted-foreground/60 group/item hover:text-white transition-colors">
                    <div class="h-1.5 w-1.5 rounded-full border border-white/20"></div>
                    <span>Nürburgring Langstrecken-Serie (NLS)</span>
                </div>
                <div class="flex items-center gap-3 text-sm text-muted-foreground/60 group/item hover:text-white transition-colors">
                    <div class="h-1.5 w-1.5 rounded-full border border-white/20"></div>
                    <span>Nürburgring 24h</span>
                </div>
                <div class="flex items-center gap-3 text-sm text-muted-foreground/60 group/item hover:text-white transition-colors">
                    <div class="h-1.5 w-1.5 rounded-full border border-white/20"></div>
                    <span>Porsche Carrera Cup</span>
                </div>
                <div class="flex items-center gap-3 text-sm text-muted-foreground/60 group/item hover:text-white transition-colors">
                    <div class="h-1.5 w-1.5 rounded-full border border-white/20"></div>
                    <span>24 Hours of Le Mans</span>
                </div>
            </div>
        </div>

        <div class="mt-6 pt-6 border-t border-white/5 flex justify-between items-center text-xs font-mono text-muted-foreground">
            <span>Status: Training</span>
            <span class="text-orange-500">Telemetry Active</span>
        </div>
    </div>
</TactileCard>