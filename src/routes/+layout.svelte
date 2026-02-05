<script lang="ts">
    import "./layout.css";
    import favicon from "$lib/assets/favicon.svg";
    import { onMount } from "svelte";
    import Lenis from "lenis";

    let { children } = $props();

    onMount(() => {
        const lenis = new Lenis({
            duration: 0.5,
            easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
            orientation: "vertical",
            gestureOrientation: "vertical",
            smoothWheel: true,
            wheelMultiplier: 0.8,
            touchMultiplier: 2,
        });

        function raf(time: number) {
            lenis.raf(time);
            requestAnimationFrame(raf);
        }

        requestAnimationFrame(raf);

        return () => {
            lenis.destroy();
        };
    });
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>
{@render children()}
