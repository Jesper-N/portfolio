<script lang="ts">
    import { onMount } from "svelte";
    import { spring } from "svelte/motion";

    let coords = spring(
        { x: 0, y: 0 },
        {
            stiffness: 0.5,
            damping: 0.7,
        },
    );

    let isVisible = $state(false);
    let isHoveringLink = $state(false);

    onMount(() => {
        const handleMouseMove = (e: MouseEvent) => {
            coords.set({ x: e.clientX, y: e.clientY });
            isVisible = true;

            // Check if hovering over clickable
            const target = e.target as HTMLElement;
            isHoveringLink = !!target.closest('a, button, [role="button"]');
        };

        const handleMouseLeave = () => {
            isVisible = false;
        };

        window.addEventListener("mousemove", handleMouseMove);
        document.body.addEventListener("mouseleave", handleMouseLeave);

        return () => {
            window.removeEventListener("mousemove", handleMouseMove);
            document.body.removeEventListener("mouseleave", handleMouseLeave);
        };
    });
</script>

<div
    class="pointer-events-none fixed top-0 left-0 z-[10000] mix-blend-difference hidden md:block will-change-transform"
    style="transform: translate({$coords.x}px, {$coords.y}px); opacity: {isVisible
        ? 1
        : 0};"
>
    <!-- Core Dot -->
    <div
        class="absolute top-0 left-0 h-2.5 w-2.5 -translate-x-1/2 -translate-y-1/2 rounded-full bg-white transition-all duration-300"
        class:scale-[3]={isHoveringLink}
    ></div>

    <!-- Outer Ring (Text Reader) -->
    <div
        class="absolute top-0 left-0 h-8 w-8 -translate-x-1/2 -translate-y-1/2 rounded-full border border-white/40 transition-all duration-500"
        class:scale-0={isHoveringLink}
        class:opacity-0={isHoveringLink}
    ></div>
</div>

<style>
    /* Global cursor hide */
    :global(body),
    :global(a),
    :global(button) {
        cursor: none !important;
    }
</style>
