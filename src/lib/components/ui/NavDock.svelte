<script lang="ts">
    import {
        BriefcaseBusiness,
        Gamepad2,
        Github,
        GraduationCap,
        House,
        Linkedin,
        Mail,
        UserRound,
    } from "@lucide/svelte";
    import { onDestroy, onMount } from "svelte";
    import { spring } from "svelte/motion";

    type IconComponent = typeof House;

    interface DockLink {
        id: string;
        label: string;
        href: string;
        icon: IconComponent;
        external?: boolean;
    }

    const sectionLinks: DockLink[] = [
        { id: "home", label: "Home", href: "#top", icon: House },
        { id: "about", label: "About", href: "#about", icon: UserRound },
        {
            id: "education",
            label: "Education",
            href: "#education",
            icon: GraduationCap,
        },
        {
            id: "work",
            label: "Work",
            href: "#work",
            icon: BriefcaseBusiness,
        },
        { id: "hobbies", label: "Hobbies", href: "#hobbies", icon: Gamepad2 },
        { id: "contact", label: "Contact", href: "#contact", icon: Mail },
    ];

    const socialLinks: DockLink[] = [
        {
            id: "github",
            label: "GitHub",
            href: "https://github.com/your-username",
            icon: Github,
            external: true,
        },
        {
            id: "linkedin",
            label: "LinkedIn",
            href: "https://linkedin.com/in/your-handle",
            icon: Linkedin,
            external: true,
        },
    ];

    const BASE_WIDTH = 38;
    const MAGNIFICATION = 60;
    const EFFECT_DISTANCE = 140;
    const SPRING_CONFIG = {
        stiffness: 0.2,
        damping: 0.6,
    };
    const TOP_VISIBILITY_THRESHOLD = 90;

    let isVisible = $state(true);

    let sectionWidths = $state(sectionLinks.map(() => BASE_WIDTH));
    let socialWidths = $state(socialLinks.map(() => BASE_WIDTH));

    const sectionSprings = sectionLinks.map(() => spring(BASE_WIDTH, SPRING_CONFIG));
    const socialSprings = socialLinks.map(() => spring(BASE_WIDTH, SPRING_CONFIG));

    const unsubscribers = [
        ...sectionSprings.map((store, index) =>
            store.subscribe((value) => {
                sectionWidths[index] = value;
            }),
        ),
        ...socialSprings.map((store, index) =>
            store.subscribe((value) => {
                socialWidths[index] = value;
            }),
        ),
    ];

    const sectionNodes: Array<HTMLDivElement | undefined> = [];
    const socialNodes: Array<HTMLDivElement | undefined> = [];

    onDestroy(() => {
        unsubscribers.forEach((unsubscribe) => unsubscribe());
    });

    function registerSectionNode(node: HTMLDivElement, index: number) {
        sectionNodes[index] = node;
        return {
            destroy() {
                sectionNodes[index] = undefined;
            },
        };
    }

    function registerSocialNode(node: HTMLDivElement, index: number) {
        socialNodes[index] = node;
        return {
            destroy() {
                socialNodes[index] = undefined;
            },
        };
    }

    function clamp(value: number, min: number, max: number) {
        return Math.max(min, Math.min(value, max));
    }

    function getWidthForNode(node: HTMLDivElement | undefined, mouseX: number) {
        if (!node || !Number.isFinite(mouseX)) return BASE_WIDTH;

        const rect = node.getBoundingClientRect();
        const distanceFromCenter = mouseX - rect.x - rect.width / 2;
        const distance = Math.abs(
            clamp(distanceFromCenter, -EFFECT_DISTANCE, EFFECT_DISTANCE),
        );
        const influence = 1 - distance / EFFECT_DISTANCE;

        return BASE_WIDTH + (MAGNIFICATION - BASE_WIDTH) * influence;
    }

    function syncWidths(mouseX: number) {
        sectionNodes.forEach((node, index) => {
            const store = sectionSprings[index];
            store?.set(getWidthForNode(node, mouseX));
        });

        socialNodes.forEach((node, index) => {
            const store = socialSprings[index];
            store?.set(getWidthForNode(node, mouseX));
        });
    }

    function handleMouseMove(event: MouseEvent) {
        syncWidths(event.pageX);
    }

    function handleMouseLeave() {
        syncWidths(Number.POSITIVE_INFINITY);
    }

    onMount(() => {
        let rafId = 0;
        let isTicking = false;
        let lastScroll = window.scrollY;

        const updateVisibility = () => {
            const currentScroll = window.scrollY;
            const nextVisible =
                currentScroll < lastScroll || currentScroll < TOP_VISIBILITY_THRESHOLD;

            if (nextVisible !== isVisible) {
                isVisible = nextVisible;
            }

            lastScroll = currentScroll;
            isTicking = false;
        };

        const handleScroll = () => {
            if (isTicking) return;
            isTicking = true;
            rafId = requestAnimationFrame(updateVisibility);
        };

        window.addEventListener("scroll", handleScroll, { passive: true });

        return () => {
            cancelAnimationFrame(rafId);
            window.removeEventListener("scroll", handleScroll);
        };
    });
</script>

<div
    class="fixed inset-x-0 bottom-5 z-50 flex justify-center px-3 transition-all duration-300 sm:bottom-8 {isVisible
        ? 'translate-y-0 opacity-100'
        : 'translate-y-24 opacity-0 pointer-events-none'}"
>
    <nav
        data-slot="nav-dock"
        onmousemove={handleMouseMove}
        onmouseleave={handleMouseLeave}
        class="mx-auto mt-8 flex h-[58px] w-max items-center gap-2 rounded-2xl border border-white/12 bg-black/60 p-2 shadow-[0_18px_44px_-22px_rgba(0,0,0,0.9)] supports-backdrop-blur:bg-white/10 supports-backdrop-blur:dark:bg-black/10 backdrop-blur-md"
        aria-label="Dock navigation"
    >
        {#each sectionLinks as link, index (link.id)}
            {@const Icon = link.icon}
            <div
                use:registerSectionNode={index}
                class="flex aspect-square cursor-pointer items-center justify-center rounded-full"
                style={`width: ${sectionWidths[index]}px;`}
            >
                <a
                    href={link.href}
                    aria-label={link.label}
                    class="group relative flex size-full items-center justify-center rounded-full p-3 text-white/90 transition-all duration-200 hover:bg-zinc-900/80 hover:text-white focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring/70 focus-visible:ring-offset-2 focus-visible:ring-offset-black"
                >
                    <Icon size={22} strokeWidth={1.2} />
                    <span
                        class="pointer-events-none absolute -top-9 left-1/2 -translate-x-1/2 rounded-md bg-black px-2 py-1 text-[10px] font-medium text-white opacity-0 transition-all duration-200 group-hover:-translate-y-1 group-hover:opacity-100 group-focus-visible:-translate-y-1 group-focus-visible:opacity-100"
                    >
                        {link.label}
                    </span>
                </a>
            </div>
        {/each}

        <div class="h-full w-[0.6px] bg-white/20"></div>

        {#each socialLinks as link, index (link.id)}
            {@const Icon = link.icon}
            <div
                use:registerSocialNode={index}
                class="flex aspect-square cursor-pointer items-center justify-center rounded-full"
                style={`width: ${socialWidths[index]}px;`}
            >
                <a
                    href={link.href}
                    target={link.external ? "_blank" : undefined}
                    rel={link.external ? "noreferrer noopener" : undefined}
                    aria-label={link.label}
                    class="group relative flex size-full items-center justify-center rounded-full p-3 text-white/90 transition-all duration-200 hover:bg-zinc-900/80 hover:text-white focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring/70 focus-visible:ring-offset-2 focus-visible:ring-offset-black"
                >
                    <Icon size={21} strokeWidth={1.6} />
                    <span
                        class="pointer-events-none absolute -top-9 left-1/2 -translate-x-1/2 rounded-md bg-black px-2 py-1 text-[10px] font-medium text-white opacity-0 transition-all duration-200 group-hover:-translate-y-1 group-hover:opacity-100 group-focus-visible:-translate-y-1 group-focus-visible:opacity-100"
                    >
                        {link.label}
                    </span>
                </a>
            </div>
        {/each}
    </nav>
</div>
