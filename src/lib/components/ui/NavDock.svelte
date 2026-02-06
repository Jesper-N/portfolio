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
        {
            id: "contact",
            label: "Contact",
            href: "mailto:hello@jesper.dev",
            icon: Mail,
            external: true,
        },
    ];

    const BASE_WIDTH = 40;
    const MAGNIFICATION = 64;
    const EFFECT_DISTANCE = 160;
    const SPRING_CONFIG = {
        stiffness: 0.15,
        damping: 0.5,
    };
    const TOP_VISIBILITY_THRESHOLD = 90;

    let isVisible = $state(false);

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
        let hasSeenFirstScrollUp = false;

        const updateVisibility = () => {
            const currentScroll = window.scrollY;
            const isScrollingUp = currentScroll < lastScroll;

            if (!hasSeenFirstScrollUp && isScrollingUp) {
                hasSeenFirstScrollUp = true;
            }

            const nextVisible =
                hasSeenFirstScrollUp &&
                (isScrollingUp || currentScroll < TOP_VISIBILITY_THRESHOLD);

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
    class="fixed inset-x-0 bottom-6 z-50 hidden md:flex justify-center px-3 transition-all duration-500 ease-[cubic-bezier(0.32,0.72,0,1)] sm:bottom-10 {isVisible
        ? 'translate-y-0 opacity-100'
        : 'translate-y-32 opacity-0 pointer-events-none'}"
>
    <nav
        data-slot="nav-dock"
        onmousemove={handleMouseMove}
        onmouseleave={handleMouseLeave}
        class="
            mx-auto mt-8 flex h-[68px] w-max items-center gap-2 rounded-full 
            border border-white/[0.08] bg-[#050505]/60 px-3 
            shadow-[0_20px_40px_-10px_rgba(0,0,0,0.5),inset_0_1px_0_rgba(255,255,255,0.1)] 
            backdrop-blur-xl supports-backdrop-blur:bg-black/40
            relative overflow-hidden
        "
        aria-label="Dock navigation"
    >
        <!-- Shimmer effect -->
        <div class="absolute inset-0 -translate-x-full animate-[shimmer_8s_infinite] bg-gradient-to-r from-transparent via-white/[0.03] to-transparent pointer-events-none"></div>

        {#each sectionLinks as link, index (link.id)}
            {@const Icon = link.icon}
            <div
                use:registerSectionNode={index}
                class="flex aspect-square cursor-pointer items-center justify-center rounded-full relative z-10"
                style={`width: ${sectionWidths[index]}px;`}
            >
                <a
                    href={link.href}
                    aria-label={link.label}
                    class="
                        group relative flex size-full items-center justify-center rounded-full p-2.5 
                        text-white/60 transition-all duration-200 
                        hover:bg-white/[0.08] hover:text-white 
                        focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-white/20
                    "
                >
                    <Icon class="w-full h-full" strokeWidth={1.5} />
                    <span
                        class="
                            pointer-events-none absolute -top-12 left-1/2 -translate-x-1/2 
                            rounded-lg border border-white/10 bg-[#0a0a0a]/90 px-3 py-1.5 
                            text-[10px] font-mono font-medium tracking-wide text-white opacity-0 blur-sm
                            transition-all duration-200 
                            group-hover:-translate-y-1 group-hover:opacity-100 group-hover:blur-0
                            before:absolute before:inset-x-0 before:-bottom-1.5 before:mx-auto before:h-1.5 before:w-1.5 before:rotate-45 before:bg-[#0a0a0a]/90 before:border-r before:border-b before:border-white/10
                        "
                    >
                        {link.label}
                    </span>
                </a>
            </div>
        {/each}

        <div class="h-8 w-[1px] bg-white/10 mx-1"></div>

        {#each socialLinks as link, index (link.id)}
            {@const Icon = link.icon}
            <div
                use:registerSocialNode={index}
                class="flex aspect-square cursor-pointer items-center justify-center rounded-full relative z-10"
                style={`width: ${socialWidths[index]}px;`}
            >
                <a
                    href={link.href}
                    target={link.external ? "_blank" : undefined}
                    rel={link.external ? "noreferrer noopener" : undefined}
                    aria-label={link.label}
                    class="
                        group relative flex size-full items-center justify-center rounded-full p-2.5 
                        text-white/60 transition-all duration-200 
                        hover:bg-white/[0.08] hover:text-white 
                        focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-white/20
                    "
                >
                    <Icon class="w-full h-full" strokeWidth={1.5} />
                    <span
                        class="
                             pointer-events-none absolute -top-12 left-1/2 -translate-x-1/2 
                            rounded-lg border border-white/10 bg-[#0a0a0a]/90 px-3 py-1.5 
                            text-[10px] font-mono font-medium tracking-wide text-white opacity-0 blur-sm
                            transition-all duration-200 
                            group-hover:-translate-y-1 group-hover:opacity-100 group-hover:blur-0
                            before:absolute before:inset-x-0 before:-bottom-1.5 before:mx-auto before:h-1.5 before:w-1.5 before:rotate-45 before:bg-[#0a0a0a]/90 before:border-r before:border-b before:border-white/10
                        "
                    >
                        {link.label}
                    </span>
                </a>
            </div>
        {/each}
    </nav>
</div>
