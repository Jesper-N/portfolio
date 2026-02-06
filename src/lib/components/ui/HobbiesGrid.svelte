<script lang="ts">
    import SimRacing from "./SimRacing.svelte";
    import Esports from "./Esports.svelte";

    let mouseX = $state<number | null>(null);
    let mouseY = $state<number | null>(null);
    const SIM_RACING_COLOR = "rgba(249, 115, 22, 0.15)";
    const ESPORTS_COLOR = "rgba(59, 130, 246, 0.15)";

    function handleMouseMove(e: MouseEvent) {
        const target = e.target as HTMLElement | null;
        const isOverCard = target?.closest(".sim-racing-card, .esports-card");

        if (!isOverCard) {
            if (mouseX !== null || mouseY !== null) {
                mouseX = null;
                mouseY = null;
            }
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
    class="grid grid-cols-1 md:grid-cols-2 gap-6 md:h-[600px] h-auto"
    onmousemove={handleMouseMove}
    onmouseleave={handleMouseLeave}
    role="presentation"
>
    
    <div class="sim-racing-card h-full">
        <SimRacing {mouseX} {mouseY} spotlightColor={SIM_RACING_COLOR} />
    </div>
    
    <div class="esports-card h-full">
        <Esports {mouseX} {mouseY} spotlightColor={ESPORTS_COLOR} />
    </div>
</div>
