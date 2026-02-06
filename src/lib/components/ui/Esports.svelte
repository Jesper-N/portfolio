<script lang="ts">
    import TactileCard from "./TactileCard.svelte";
    import CardSpotlight from "./CardSpotlight.svelte";

    let {
        mouseX = null,
        mouseY = null,
        spotlightColor = "rgba(59, 130, 246, 0.06)",
    } = $props();
</script>

<TactileCard class="p-8 h-full relative group bg-[#0a0a0a] overflow-visible">
    <div class="absolute inset-0 z-0">
        <CardSpotlight color={spotlightColor} {mouseX} {mouseY} />
    </div>

    <div class="relative z-10 flex flex-col h-full pointer-events-none">
        <div class="flex items-center gap-2 mb-6">
            <span class="h-2 w-2 rounded-full bg-blue-500"></span>
            <span
                class="font-mono text-xs uppercase tracking-widest text-muted-foreground"
                >Competitive Esports</span
            >
        </div>

        <h3 class="text-2xl font-medium text-white mb-2">
            Highest Rank Achieved
        </h3>
        <p
            class="text-xs font-mono text-muted-foreground uppercase tracking-widest mb-6 border-b border-white/5 pb-6"
        >
            Across all FPS titles played
        </p>


        <div class="pointer-events-auto">
            <div class="space-y-4">

                <div
                    class="flex items-center justify-between border-b border-white/5 pb-2"
                >
                    <span class="text-sm text-muted-foreground"
                        >KovaaKs Records</span
                    >
                    <span class="font-mono text-lg text-white">50+ WRs</span>
                </div>
                <div
                    class="flex items-center justify-between border-b border-white/5 pb-2"
                >
                    <span class="text-sm text-muted-foreground"
                        >Aim Mechanics</span
                    >
                    <span class="font-mono text-lg text-white"
                        >Top 10 World</span
                    >
                </div>
                <div
                    class="flex items-center justify-between border-b border-white/5 pb-2"
                >
                    <span class="text-sm text-muted-foreground">Main Role</span>
                    <span class="font-mono text-sm text-white uppercase"
                        >Hitscan / DPS</span
                    >
                </div>
                <div
                    class="flex items-center justify-between border-b border-white/5 pb-2"
                >
                    <span class="text-sm text-muted-foreground"
                        >Tournament Wins</span
                    >
                    <span class="font-mono text-lg text-white">15+</span>
                </div>
                <div
                    class="flex items-center justify-between border-b border-white/5 pb-2"
                >
                    <span class="text-sm text-muted-foreground"
                        >Podium Finishes</span
                    >
                    <span class="font-mono text-lg text-white">30+</span>
                </div>
            </div>
        </div>

        <div class="mt-auto pt-6 border-t border-white/5">
            <p class="text-sm text-muted-foreground leading-relaxed">
                2 years of professional competition. Leveraging elite mechanical
                precision and reaction speeds into software optimization.
            </p>
        </div>
    </div>
</TactileCard>
