<script lang="ts">
    import { onMount } from "svelte";
    import TactileCard from "./TactileCard.svelte";
    import { cn } from "$lib/utils.js";

    const skills = [
        {
            category: "Core",
            items: ["Svelte", "SvelteKit", "TypeScript", "Tailwind CSS"],
            color: "text-blue-400"
        },
        {
            category: "Backend",
            items: ["Node.js", "Express", "PHP", "PostgreSQL"],
            color: "text-emerald-400"
        },
        {
            category: "Systems",
            items: ["Embedded C", "Rust", "Linux", "Docker"],
            color: "text-orange-400"
        },
        {
            category: "AI/Data",
            items: ["OpenAI API", "Claude API", "TensorFlow", "Python"],
            color: "text-purple-400"
        }
    ];

    let mounted = $state(false);

    onMount(() => {
        mounted = true;
    });
</script>

<TactileCard class="min-h-75 flex flex-col relative overflow-hidden group">
    <!-- Ambient Background Glow -->
    <div class="absolute -top-24 -right-24 w-64 h-64 bg-white/3 rounded-full blur-3xl pointer-events-none transition-opacity duration-1000 group-hover:opacity-100 opacity-50"></div>
    <div class="absolute -bottom-24 -left-24 w-64 h-64 bg-white/2 rounded-full blur-3xl pointer-events-none"></div>

    <!-- Header / Title Bar -->
    <div class="relative z-10 flex items-center justify-between p-6 pb-4 border-b border-white/5 bg-white/1">
        <div class="flex items-center gap-3">
             <div class="flex gap-2">
                 <div class="h-2.5 w-2.5 rounded-full bg-[#FF5F56] shadow-[0_0_8px_rgba(255,95,86,0.3)] border border-[#FF5F56]/20"></div>
                 <div class="h-2.5 w-2.5 rounded-full bg-[#FFBD2E] shadow-[0_0_8px_rgba(255,189,46,0.3)] border border-[#FFBD2E]/20"></div>
                 <div class="h-2.5 w-2.5 rounded-full bg-[#27C93F] shadow-[0_0_8px_rgba(39,201,63,0.3)] border border-[#27C93F]/20"></div>
             </div>
             <div class="h-4 w-px bg-white/10 mx-1"></div>
             <div class="flex items-center gap-2 font-mono text-xs text-muted-foreground/80">
                <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="opacity-50"><polyline points="4 7 4 4 20 4 20 7"></polyline><line x1="9" x2="15" y1="20" y2="20"></line><line x1="12" x2="12" y1="4" y2="20"></line></svg>
                <span class="tracking-tight">TECH_STACK<span class="opacity-50">.json</span></span>
             </div>
        </div>
        <div class="flex gap-1 opacity-0 group-hover:opacity-100 transition-opacity duration-500 delay-100">
            <div class="h-1 w-1 rounded-full bg-white/20"></div>
            <div class="h-1 w-1 rounded-full bg-white/20"></div>
            <div class="h-1 w-1 rounded-full bg-white/20"></div>
        </div>
    </div>

    <!-- Content Area -->
    <div class="relative z-10 p-6 pt-6 grid grid-cols-2 gap-y-8 gap-x-4 h-full">
        {#each skills as group, i}
            <div
                class="space-y-3 transition-all duration-700 ease-out transform"
                class:opacity-0={!mounted}
                class:translate-y-4={!mounted}
                style="transition-delay: {i * 100}ms"
            >
                <div class="flex items-center gap-2">
                    <span class={cn("text-[10px] font-mono", group.color)}>//</span>
                    <h4 class="text-[10px] font-mono uppercase tracking-[0.2em] text-muted-foreground font-semibold">
                        {group.category}
                    </h4>
                    <div class="h-px bg-white/5 flex-1 origin-left scale-x-0 transition-transform duration-700 delay-500 ease-out" class:scale-x-100={mounted}></div>
                </div>

                <ul class="space-y-1.5 relative group/list">
                    <!-- Vertical decoration line -->
                    <div class="absolute left-0.75 top-1 bottom-1 w-px bg-linear-to-b from-white/5 via-white/5 to-transparent"></div>

                    {#each group.items as item}
                        <li class="group/item relative pl-4 text-sm text-muted-foreground hover:text-white transition-colors duration-200 flex items-center">
                            <!-- Hover scanning highlight -->
                            <div class="absolute left-0 top-0 bottom-0 w-px bg-white opacity-0 group-hover/item:opacity-100 transition-all duration-200 h-0 group-hover/item:h-full box-shadow-[0_0_8px_white]"></div>

                            <!-- Hover background highlight -->
                            <div class="absolute inset-0 -left-2 -right-2 bg-white/2 opacity-0 group-hover/item:opacity-100 rounded-sm transition-opacity duration-200 pointer-events-none"></div>

                            <span class="relative z-10 transition-transform duration-200 group-hover/item:translate-x-1 font-light tracking-wide font-mono text-xs">
                                <span class="opacity-0 group-hover/item:opacity-50 text-[10px] mr-1 transition-opacity duration-200">"</span>{item}<span class="opacity-0 group-hover/item:opacity-50 text-[10px] ml-1 transition-opacity duration-200">",</span>
                            </span>
                        </li>
                    {/each}
                </ul>
            </div>
        {/each}
    </div>

    <!-- Status Bar / Footer -->
    <div class="relative z-10 mt-auto border-t border-white/5 bg-white/1 px-4 py-2 flex justify-between items-center text-[10px] font-mono text-muted-foreground/40">
        <div class="flex gap-4">
             <span class="flex items-center gap-1.5 hover:text-white/60 transition-colors">
                <span class="h-1.5 w-1.5 rounded-full bg-blue-500/50 animate-pulse"></span>
                <span>TS 5.3</span>
             </span>
             <span class="hover:text-white/60 transition-colors">UTF-8</span>
             <span class="hover:text-white/60 transition-colors">JSON</span>
        </div>
        <div class="flex items-center gap-2">
            <span>Ln {skills.reduce((acc, g) => acc + g.items.length + 2, 0)}, Col 1</span>
            <div class="h-3 w-1.5 bg-white/20 animate-pulse"></div>
        </div>
    </div>
</TactileCard>
