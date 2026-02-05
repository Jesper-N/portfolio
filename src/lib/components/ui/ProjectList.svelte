<script lang="ts">
    import { cn } from "$lib/utils";
    
    let { 
        projects = [] 
    }: {
        projects: Array<{
            id: string;
            title: string;
            category: string;
            year: string;
            image: string;
        }>
    } = $props();

    let hoveredIndex = $state<number | null>(null);
    let cursorX = $state(0);
    let cursorY = $state(0);

    function handleMouseMove(e: MouseEvent) {
        cursorX = e.clientX;
        cursorY = e.clientY;
    }
</script>

<div 
    class="relative w-full py-20"
    onmousemove={handleMouseMove}
    role="list"
>
    <!-- Floating Image Preview -->
    {#if hoveredIndex !== null}
        <div 
            class="pointer-events-none fixed z-20 h-[400px] w-[300px] overflow-hidden rounded-sm object-cover opacity-0 transition-opacity duration-500 md:opacity-100"
            style="
                left: {cursorX}px; 
                top: {cursorY}px; 
                transform: translate(-50%, -50%) rotate({(cursorX - window.innerWidth/2) / 50}deg);
            "
        >
            <img 
                src={projects[hoveredIndex].image} 
                alt="Project Preview" 
                class="h-full w-full object-cover grayscale transition-all duration-700 hover:grayscale-0"
            />
            <!-- Overlay Info -->
            <div class="absolute bottom-0 left-0 w-full bg-black/50 p-4 backdrop-blur-md">
                 <span class="font-mono text-xs uppercase text-white tracking-widest">
                     {projects[hoveredIndex].category} // {projects[hoveredIndex].year}
                 </span>
            </div>
        </div>
    {/if}

    <!-- List Items -->
    <div class="flex flex-col">
        {#each projects as project, i}
            <a 
                href="/project/{project.id}"
                class="group relative flex items-baseline justify-between border-t border-white/10 py-12 px-4 transition-colors hover:bg-white/5 md:px-12"
                onmouseenter={() => hoveredIndex = i}
                onmouseleave={() => hoveredIndex = null}
            >
                <div class="relative z-10 flex items-baseline gap-8 transition-transform duration-500 group-hover:translate-x-4">
                    <span class="font-mono text-xs text-muted-foreground">0{i + 1}</span>
                    <h2 class="font-sans text-5xl font-light tracking-tighter text-foreground transition-colors group-hover:text-white md:text-7xl lg:text-8xl">
                        {project.title}
                    </h2>
                </div>
                
                <div class="relative z-10 hidden items-center gap-12 font-mono text-xs uppercase tracking-widest text-muted-foreground md:flex">
                    <span class="transition-colors group-hover:text-white">{project.category}</span>
                    <span class="transition-colors group-hover:text-white">{project.year}</span>
                    <span class="inline-block transition-transform duration-300 group-hover:translate-x-2 group-hover:text-primary">-></span>
                </div>
            </a>
        {/each}
        <div class="border-t border-white/10"></div>
    </div>
</div>