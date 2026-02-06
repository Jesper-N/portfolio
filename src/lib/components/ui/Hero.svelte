<script lang="ts">
    import TactileCard from "$lib/components/ui/TactileCard.svelte";
    import { Button } from "$lib/components/ui/button/index.js";
</script>

<div class="space-y-4">
    <h1 class="font-sans text-6xl md:text-8xl font-medium tracking-tighter text-white">
       Jesper Nielsen
   </h1>
   <p class="font-mono text-sm md:text-base text-muted-foreground pt-4 max-w-xl">
       FULL STACK DEVELOPER
   </p>
</div>

<TactileCard class="p-8 md:p-12 relative overflow-hidden group border-white/10 mt-12 bg-white/[0.02]">
    
    <div class="absolute inset-0 bg-gradient-to-br from-white/[0.05] via-transparent to-transparent pointer-events-none"></div>

    <div class="relative z-10 grid grid-cols-1 md:grid-cols-2 gap-12 items-start">
        <div class="space-y-6">
             <h2 class="text-3xl font-medium text-white tracking-tight">
                Data Technician<br><span class="text-muted-foreground">Specializing in Programming.</span>
             </h2>
             <p class="text-muted-foreground leading-relaxed">
                 I specialize in full-stack web development, but I've never stayed in one lane. I've trained custom AI models, written firmware, built mobile apps, engineered complex scraping pipelines, and automated entire workflows. This breadth allows me to architect and build complete, end-to-end systems without the friction or overhead of coordinating multiple teams
              </p>
             <div class="flex gap-4 pt-4">
                <Button href="#work" variant="pill" class="rounded-full">
                    View Work
                </Button>
                <Button href="mailto:hello@jesper.dev" variant="outline" class="rounded-full border-white/20 hover:bg-white/5">
                    Contact
                </Button>
             </div>
        </div>

        
        <div class="hidden md:flex justify-end items-center h-full opacity-20">
             <div class="h-32 w-32 rounded-full border border-white/20 relative animate-[spin_10s_linear_infinite]">
                <div class="absolute top-0 left-1/2 -translate-x-1/2 w-1 h-1 bg-white rounded-full"></div>
             </div>
        </div>
    </div>
</TactileCard>
