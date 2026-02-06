<script lang="ts">
    import { spring } from "svelte/motion";

    let {
        color = "rgba(255, 255, 255)",
        mouseX = null,
        mouseY = null,
    } = $props();

    let div: HTMLDivElement;
    let opacity = $state(0);
    let coords = spring(
        { x: 0, y: 0 },
        {
            stiffness: 0.15,
            damping: 0.7,
        },
    );
    function handleLocalMouseMove(e: MouseEvent) {
        if (mouseX !== null) return;

        if (!div) return;
        const rect = div.getBoundingClientRect();
        coords.set({
            x: e.clientX - rect.left,
            y: e.clientY - rect.top,
        });
        opacity = 1;
    }

    function handleLocalMouseLeave() {
        if (mouseX !== null) return;
        opacity = 0;
    }
    $effect(() => {
        if (div && mouseX !== null && mouseY !== null) {
            const rect = div.getBoundingClientRect();
            coords.set({
                x: mouseX - rect.left,
                y: mouseY - rect.top,
            });
            opacity = 1;
        } else if (mouseX === null || mouseY === null) {
            opacity = 0;
        }
    });
</script>

<div
    bind:this={div}
    onmousemove={handleLocalMouseMove}
    onmouseleave={handleLocalMouseLeave}
    class="absolute inset-0 z-0 overflow-hidden rounded-xl bg-transparent"
    role="presentation"
>
    <div
        class="pointer-events-none absolute -inset-px transition-opacity duration-500 ease-out"
        style="
            opacity: {opacity};
            background: radial-gradient(
                550px circle at {$coords.x}px {$coords.y}px,
                {color},
                transparent 50%
            );
        "
    ></div>
</div>
