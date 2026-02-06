<script lang="ts">
    import { onMount } from "svelte";

    const links = [
        { label: "Index", href: "#top" },
        { label: "Work", href: "#work" },
        { label: "About", href: "#about" },
        { label: "Contact", href: "mailto:hello@portfolio.com" }
    ];

    let show = $state(true);
    let lastScroll = 0;

    onMount(() => {
        let rafId = 0;
        let ticking = false;

        const updateVisibility = () => {
            const currentScroll = window.scrollY;
            const nextShow = currentScroll < lastScroll || currentScroll < 100;

            if (nextShow !== show) {
                show = nextShow;
            }

            lastScroll = currentScroll;
            ticking = false;
        };

        const handleScroll = () => {
            if (ticking) return;
            ticking = true;
            rafId = requestAnimationFrame(updateVisibility);
        };

        lastScroll = window.scrollY;
        window.addEventListener("scroll", handleScroll, { passive: true });

        return () => {
            cancelAnimationFrame(rafId);
            window.removeEventListener("scroll", handleScroll);
        };
    });
</script>

<div class="fixed bottom-8 left-1/2 z-50 -translate-x-1/2 transition-all duration-500 {show ? 'translate-y-0 opacity-100' : 'translate-y-24 opacity-0'}">
    <nav class="flex items-center gap-1 rounded-full bg-[#111] p-1.5 shadow-2xl ring-1 ring-white/10 backdrop-blur-md">
        {#each links as link}
            <a 
                href={link.href}
                class="relative rounded-full px-6 py-2.5 font-mono text-xs uppercase tracking-wider text-muted-foreground transition-all hover:bg-white/10 hover:text-white active:scale-95"
            >
                {link.label}
            </a>
        {/each}
        <div class="mx-2 h-4 w-[1px] bg-white/10"></div>
        <div class="px-4 font-mono text-[10px] text-muted-foreground/50">
            ©2026
        </div>
    </nav>
</div>
