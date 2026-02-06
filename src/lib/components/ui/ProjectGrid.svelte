<script lang="ts">
    import TactileCard from "./TactileCard.svelte";
    import { Button } from "$lib/components/ui/button/index.js";

    let {
        projects = [],
    }: {
        projects: Array<{
            id: string;
            title: string;
            description: string;
            tags: string[];
            image: string;
            year: string;
        }>;
    } = $props();
</script>

<div class="grid grid-cols-1 md:grid-cols-2 gap-6 w-full">
    {#each projects as project (project.id)}
        <TactileCard
            href="/project/{project.id}"
            interactive={true}
            class="h-[400px] flex flex-col p-2"
        >
            
            <div
                class="relative h-[240px] w-full overflow-hidden rounded-lg bg-[#111111]"
            >
                <img
                    src={project.image}
                    alt={project.title}
                    class="h-full w-full object-cover transition-all duration-700 group-hover:scale-105 opacity-80 group-hover:opacity-100 grayscale group-hover:grayscale-0"
                />

                
                <div
                    class="absolute top-3 right-3 rounded-full bg-black/60 backdrop-blur-md px-3 py-1 text-[10px] font-mono uppercase tracking-widest text-white/80 border border-white/10"
                >
                    {project.year}
                </div>
            </div>

            
            <div class="flex flex-1 flex-col justify-between p-4 pt-6">
                <div class="space-y-2">
                    <h3
                        class="font-sans text-2xl font-medium tracking-tight text-white group-hover:text-white/90"
                    >
                        {project.title}
                    </h3>
                    <p
                        class="text-sm text-muted-foreground line-clamp-2 leading-relaxed"
                    >
                        {project.description}
                    </p>
                </div>

                <div class="flex flex-wrap gap-2 pt-4">
                    {#each project.tags as tag (`${project.id}-${tag}`)}
                        <Button
                            variant="tag"
                            size="tag"
                            class="font-mono uppercase tracking-wider"
                        >
                            {tag}
                        </Button>
                    {/each}
                </div>
            </div>
        </TactileCard>
    {/each}
</div>
