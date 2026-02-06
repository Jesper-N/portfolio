<script lang="ts">
    import { cn } from "$lib/utils.js";
    
    let { 
        children, 
        class: className = "",
        interactive = false,
        href = null,
        style = ""
    } = $props();
</script>

{#if href}
    <a 
        {href}
        class={cn(
            "group relative flex flex-col rounded-xl border border-white/10 bg-card text-card-foreground transition-all duration-300",
            interactive && "hover:border-white/20 hover:bg-card/80",
            className
        )}
        {style}
    >
        {@render children()}
    </a>
{:else}
    <div 
        class={cn(
            "relative flex flex-col rounded-xl border border-white/10 bg-card text-card-foreground",
            interactive && "transition-all duration-300 hover:border-white/20 hover:bg-card/80",
            className
        )}
        {style}
    >
        {@render children()}
    </div>
{/if}
