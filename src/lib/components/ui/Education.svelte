<script lang="ts">
    import TactileCard from './TactileCard.svelte';
</script>

<div class="space-y-12">
    <div class="flex items-baseline justify-between border-b border-white/5 pb-4">
        <h2 class="text-3xl font-medium text-white tracking-tight">Education</h2>
        <span class="font-mono text-xs text-muted-foreground">CERTIFICATION</span>
    </div>

    <TactileCard class="p-8 md:p-12 relative overflow-hidden group">

        <div class="absolute right-0 top-0 h-full w-1/3 bg-linear-to-l from-white/5 to-transparent pointer-events-none"></div>
        <div class="absolute -right-12 -top-12 h-64 w-64 rounded-full bg-white/5 blur-3xl group-hover:bg-white/10 max-md:bg-white/10 transition-colors duration-700"></div>

        <div class="relative z-10 flex flex-col md:flex-row justify-between items-start md:items-center gap-8">
            <div class="space-y-4">
                <div class="flex items-center gap-3">
                    <span class="px-2 py-1 rounded border border-white/10 bg-white/5 text-[10px] font-mono uppercase tracking-widest text-muted-foreground">
                        2019 — 2025
                    </span>
                    <span class="h-px w-8 bg-white/10"></span>
                    <span class="text-xs font-mono text-muted-foreground uppercase tracking-widest">
                        Mercantec Viborg
                    </span>
                </div>

                <div>
                    <h3 class="text-3xl md:text-4xl font-medium text-white mb-2">
                        Data Technician
                    </h3>
                    <p class="text-lg text-muted-foreground font-light">
                        Specializing in Programming
                    </p>
                </div>
            </div>


            <div class="flex items-center gap-8 md:gap-10">
                <div class="block md:min-w-48 text-right space-y-1.5">
                    <div class="flex items-center justify-end gap-3">
                        <span class="h-px w-6 bg-white/20 group-hover:bg-yellow-500/40 max-md:bg-yellow-500/40 transition-colors duration-500"></span>
                        <div class="text-sm font-mono text-muted-foreground uppercase tracking-widest">Final Grade</div>
                    </div>
                    <div class="text-[10px] font-mono uppercase tracking-[0.2em] text-muted-foreground/60 group-hover:text-yellow-300/70 max-md:text-yellow-300/70 transition-colors duration-500">
                        Danish 7-Step Scale
                    </div>
                </div>

                <div class="relative flex items-center justify-center h-20 w-20 rounded-full border border-white/10 bg-white/5 group-hover:border-yellow-500/50 group-hover:bg-yellow-500/10 max-md:border-yellow-500/50 max-md:bg-yellow-500/10 transition-colors duration-500">
                    <div class="absolute inset-0 rounded-full border border-white/5 scale-110"></div>
                    <span class="font-sans text-4xl font-bold text-white group-hover:text-yellow-400 max-md:text-yellow-400 transition-colors duration-500">12</span>
                    <div class="absolute -bottom-2 px-2 py-0.5 rounded-full bg-[#0a0a0a] border border-white/10 text-[10px] font-mono uppercase tracking-widest text-muted-foreground group-hover:text-yellow-400 group-hover:border-yellow-500/30 max-md:text-yellow-400 max-md:border-yellow-500/30 transition-colors">
                        Top
                    </div>
                </div>
            </div>
        </div>
    </TactileCard>
</div>
