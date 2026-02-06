<script lang="ts">
    import { Canvas } from "@threlte/core";
    import { onMount } from "svelte";
    import HeroSceneInner from "./HeroSceneInner.svelte";

    let mounted = $state(false);
    let mouseX = $state(0);
    let mouseY = $state(0);

    onMount(() => {
        mounted = true;
    });

    function handleMouseMove(e: MouseEvent) {
        const rect = (e.currentTarget as HTMLElement).getBoundingClientRect();
        mouseX = ((e.clientX - rect.left) / rect.width) * 2 - 1;
        mouseY = -((e.clientY - rect.top) / rect.height) * 2 + 1;
    }

    function handleMouseLeave() {
        mouseX = 0;
        mouseY = 0;
    }
</script>

<div
    class="absolute inset-0 z-0"
    role="presentation"
    onmousemove={handleMouseMove}
    onmouseleave={handleMouseLeave}
>
    {#if mounted}
        <Canvas>
            <HeroSceneInner {mouseX} {mouseY} />
        </Canvas>
    {/if}
</div>
