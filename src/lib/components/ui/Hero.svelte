<script lang="ts">
    import TactileCard from "$lib/components/ui/TactileCard.svelte";
    import CardSpotlight from "$lib/components/ui/CardSpotlight.svelte";
    import { Button } from "$lib/components/ui/button/index.js";
    import HeroScene from "$lib/components/ui/HeroScene.svelte";
    import { onMount } from "svelte";

    let scrollIndicatorVisible = $state(true);

    onMount(() => {
        const handleScroll = () => {
            scrollIndicatorVisible = window.scrollY < 400;
        };
        window.addEventListener("scroll", handleScroll, { passive: true });
        return () => window.removeEventListener("scroll", handleScroll);
    });
</script>

<!-- Name reveal -->
<div class="space-y-5">
    <h1 class="font-sans text-6xl md:text-[7rem] lg:text-[8.5rem] font-medium tracking-tighter text-white leading-[0.9]">
        <span class="block overflow-hidden">
            <span
                class="inline-block animate-[revealUp_0.8s_cubic-bezier(0.22,1,0.36,1)_forwards]"
                style="opacity: 0; transform: translateY(110%)"
            >Jesper</span>
        </span>
        <span class="block overflow-hidden">
            <span
                class="inline-block animate-[revealUp_0.8s_cubic-bezier(0.22,1,0.36,1)_0.15s_forwards]"
                style="opacity: 0; transform: translateY(110%)"
            >Nielsen</span>
        </span>
    </h1>

    <!-- Subtitle with line -->
    <div
        class="animate-[fadeIn_0.6s_ease_0.7s_forwards]"
        style="opacity: 0"
    >
        <div class="flex items-center gap-4">
            <p class="font-mono text-sm md:text-base text-muted-foreground tracking-widest uppercase">
                Full Stack Developer
            </p>
            <div
                class="h-px flex-1 bg-linear-to-r from-white/20 to-transparent origin-left animate-[lineGrow_0.8s_ease_0.9s_forwards]"
                style="transform: scaleX(0)"
            ></div>
        </div>

        <!-- Status indicator -->
        <div class="flex items-center gap-2.5 mt-3">
            <span class="relative flex h-2 w-2">
                <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                <span class="relative inline-flex rounded-full h-2 w-2 bg-emerald-500"></span>
            </span>
            <span class="font-mono text-xs text-emerald-500/80 tracking-wide">Available for work</span>
        </div>
    </div>
</div>

<!-- Bio card -->
<div class="relative">
    <TactileCard class="p-0 relative overflow-hidden group border-white/8 mt-14 bg-transparent animate-[fadeIn_0.8s_ease_0.5s_forwards]" style="opacity: 0">

        <!-- CardSpotlight glow effect -->
        <CardSpotlight color="rgba(160, 196, 255, 0.04)" />

        <!-- Dot grid background -->
        <div class="absolute inset-0 tactile-grid opacity-40 pointer-events-none"></div>

        <!-- Top gradient overlay -->
        <div class="absolute inset-0 bg-linear-to-br from-white/3 via-transparent to-transparent pointer-events-none"></div>

        <!-- Animated border glow accent -->
        <div class="absolute -inset-px rounded-xl opacity-0 group-hover:opacity-100 transition-opacity duration-700 pointer-events-none"
            style="background: conic-gradient(from 180deg at 50% 50%, transparent 0deg, rgba(160, 196, 255, 0.08) 60deg, transparent 120deg, rgba(202, 240, 248, 0.06) 240deg, transparent 360deg);"
        ></div>

        <div class="relative z-10 grid grid-cols-1 md:grid-cols-5 gap-0">
            <!-- Left column: text content -->
            <div class="md:col-span-3 p-8 md:p-12 space-y-6">
                <h2 class="text-3xl md:text-4xl font-medium text-white tracking-tight leading-snug">
                    Data Technician<br>
                    <span class="text-muted-foreground">Specializing in Programming.</span>
                </h2>

                <p class="text-muted-foreground leading-relaxed max-lg:leading-relaxed max-w-lg">
                    I specialize in full-stack web development, but I've never stayed in one lane. I've trained custom AI models, written firmware, built mobile apps, engineered complex scraping pipelines, and automated entire workflows. This breadth allows me to architect and build complete, end-to-end systems without the friction or overhead of coordinating multiple teams
                </p>

                <div class="flex gap-4 pt-4">
                    <Button href="#work" variant="pill" class="rounded-full group/btn relative overflow-hidden">
                        <span class="relative z-10">View Work</span>
                    </Button>
                    <Button href="mailto:hello@jesper.dev" variant="outline" class="rounded-full border-white/20 hover:bg-white/5 hover:border-white/30 transition-all duration-300">
                        Contact
                    </Button>
                </div>
            </div>

            <!-- Right column: 3D scene-->
            <div class="hidden md:block md:col-span-2 relative min-h-85">
                <!-- Subtle vertical divider -->
                <div class="absolute left-0 top-8 bottom-8 w-px bg-linear-to-b from-transparent via-white/6 to-transparent"></div>

                <!-- 3D Threlte scene -->
                <HeroScene />

                <!-- Bottom ambient glow beneath the 3D object -->
                <div class="absolute bottom-6 left-1/2 -translate-x-1/2 w-32 h-8 bg-sky-400/6 blur-2xl rounded-full pointer-events-none"></div>
            </div>


        </div>
    </TactileCard>

    <!-- Scroll indicator: absolute to float in the section gap -->
    <div
        class="absolute left-1/2 -translate-x-1/2 top-full mt-4 flex flex-col items-center gap-2 transition-opacity duration-500"
        class:opacity-0={!scrollIndicatorVisible}
        class:pointer-events-none={!scrollIndicatorVisible}
    >
        <span class="font-mono text-[10px] text-muted-foreground/50 tracking-[0.2em] uppercase">Scroll</span>
        <div class="relative w-px h-8 bg-white/6 overflow-hidden">
            <div
                class="absolute w-px h-2 bg-white/30 animate-[scrollLine_2s_ease-in-out_infinite]"
            ></div>
        </div>
    </div>
</div>
