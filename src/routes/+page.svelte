<script lang="ts">
    import ProjectGrid from "$lib/components/ui/ProjectGrid.svelte";
    import NavDock from "$lib/components/ui/NavDock.svelte";
    import InfoGrid from "$lib/components/ui/InfoGrid.svelte";
    import Education from "$lib/components/ui/Education.svelte";
    import MagneticCursor from "$lib/components/ui/MagneticCursor.svelte";
    import HobbiesGrid from "$lib/components/ui/HobbiesGrid.svelte";
    import Hero from "$lib/components/ui/Hero.svelte";
    const projects = [
        {
            id: "1",
            title: "EigenLayer Explorer",
            description:
                "A comprehensive dashboard for the EigenLayer ecosystem. Real-time TVL tracking, restaking analytics, and operator metrics using The Graph protocol.",
            tags: ["SvelteKit", "The Graph", "Web3", "Tailwind"],
            year: "2025",
            image: "https://images.unsplash.com/photo-1639322537228-f710d846310a?q=80&w=2532&auto=format&fit=crop",
        },
        {
            id: "2",
            title: "Neural Forge",
            description:
                "AI-powered workflow automation tool. Drag-and-drop interface for constructing complex LLM chains using custom Svelte stores for state management.",
            tags: ["Svelte", "Python", "OpenAI API", "Redis"],
            year: "2025",
            image: "https://images.unsplash.com/photo-1620712943543-bcc4688e7485?q=80&w=2565&auto=format&fit=crop",
        },
        {
            id: "3",
            title: "Carbon OS",
            description:
                "Embedded energy grid monitoring system. High-contrast data visualization for industrial IoT devices running on low-power hardware.",
            tags: ["Rust", "WASM", "Svelte", "MQTT"],
            year: "2024",
            image: "https://images.unsplash.com/photo-1558494949-ef526b0042a0?q=80&w=2695&auto=format&fit=crop",
        },
        {
            id: "4",
            title: "Vesper Finance",
            description:
                "DeFi yield aggregator interface. Simplified complex financial data into a clean, consumer-friendly UI with real-time WebSocket updates.",
            tags: ["TypeScript", "Ethers.js", "Vite"],
            year: "2024",
            image: "https://images.unsplash.com/photo-1642104704074-907c0698cbd9?q=80&w=2532&auto=format&fit=crop",
        },
    ];
</script>

<svelte:head>
    <title>Jesper Nielsen // Data Technician</title>
</svelte:head>

<MagneticCursor />
<NavDock />


<div class="bg-grain"></div>

<main
    id="top"
    class="relative z-10 min-h-screen max-w-6xl mx-auto px-6 pt-16 pb-32 space-y-32"
>
    
    <section>
        <Hero />
    </section>

    
    <section id="about">
        <InfoGrid />
    </section>

    
    <section id="education">
        <Education />
    </section>

    
    <section id="work" class="space-y-12">
        <div
            class="flex items-baseline justify-between border-b border-white/5 pb-4"
        >
            <h2 class="text-3xl font-medium text-white tracking-tight">
                Selected Projects
            </h2>
            <span class="font-mono text-xs text-muted-foreground">01 — 04</span>
        </div>

        <ProjectGrid {projects} />
    </section>

    
    <section id="hobbies" class="space-y-12">
        <div
            class="flex items-baseline justify-between border-b border-white/5 pb-4"
        >
            <h2 class="text-3xl font-medium text-white tracking-tight">
                Hobbies
            </h2>
            <span class="font-mono text-xs text-muted-foreground">OFFLINE</span>
        </div>

        <HobbiesGrid />
    </section>

    
    <footer id="contact" class="pt-24 border-t border-white/5">
        <div class="flex flex-col md:flex-row justify-between items-end gap-12">
            <div>
                <h1
                    class="text-[12vw] leading-none font-bold text-white/5 tracking-tighter select-none"
                >
                    NIELSEN
                </h1>
            </div>
            <div
                class="flex gap-8 mb-4 md:mb-8 font-mono text-sm text-muted-foreground"
            >
                <a href="#" class="hover:text-white transition-colors">GitHub</a
                >
                <a href="#" class="hover:text-white transition-colors"
                    >LinkedIn</a
                >
            </div>
        </div>
    </footer>
</main>
