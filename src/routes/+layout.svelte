<script lang="ts">
    import "./layout.css";
    import favicon from "$lib/assets/favicon.svg";

    let { children } = $props();
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>

<figure class="pointer-events-none fixed inset-0 z-9999 opacity-10 filter-[url(#noise-bg-fx)_grayscale(100%)]" aria-hidden="true">
    <svg>
        <filter id="noise-bg-fx">
            <feTurbulence baseFrequency="0.8"></feTurbulence>
        </filter>
    </svg>
</figure>

{@render children()}
