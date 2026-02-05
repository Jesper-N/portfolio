<script lang="ts">
    import { cn } from "$lib/utils";
    import SimRacing from "./SimRacing.svelte";
    import Esports from "./Esports.svelte";

    let container: HTMLDivElement;
    let mouseX = $state<number | null>(null);
    let mouseY = $state<number | null>(null);

    // Define colors for the sections
    const SIM_RACING_COLOR = "rgba(249, 115, 22, 0.15)"; // Orange
    const ESPORTS_COLOR = "rgba(59, 130, 246, 0.15)";    // Blue

    function handleMouseMove(e: MouseEvent) {
        const target = e.target as HTMLElement;
        
        // If we are strictly in the gap (the container itself), hide the light
        // This relies on the cards having a background/content that catches the event
        // or the gap being the container background.
        // Checking if the target is one of the card containers or inside them.
        const isOverCard = target.closest('.sim-racing-card') || target.closest('.esports-card');

        if (!isOverCard) {
            mouseX = null;
            mouseY = null;
            return;
        }

        mouseX = e.clientX;
        mouseY = e.clientY;
    }

    function handleMouseLeave() {
        mouseX = null;
        mouseY = null;
    }
</script>

<div 
    bind:this={container}
    class="grid grid-cols-1 md:grid-cols-2 gap-6 md:h-[600px] h-auto"
    onmousemove={handleMouseMove}
    onmouseleave={handleMouseLeave}
    role="presentation"
>
    <!-- 
      We pass the SHARED coordinates to both cards to allow the "bleed" effect across the edge.
      BUT we pass distinct colors to each card so they maintain their identity.
    -->
    <div class="sim-racing-card h-full">
        <SimRacing {mouseX} {mouseY} spotlightColor={SIM_RACING_COLOR} />
    </div>
    
    <div class="esports-card h-full">
        <Esports {mouseX} {mouseY} spotlightColor={ESPORTS_COLOR} />
    </div>
</div>