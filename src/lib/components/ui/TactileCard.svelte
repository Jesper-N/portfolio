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
            "group relative overflow-hidden rounded-xl bg-[#141414] border border-white/5 transition-all duration-500",
            interactive && "hover:-translate-y-1 hover:border-white/20 hover:shadow-[0_10px_40px_-10px_rgba(0,0,0,0.5)]",
            className
        )}
        {style}
    >
        
        <div class="absolute inset-0 opacity-[0.03] pointer-events-none bg-[url('https://grainy-gradients.vercel.app/noise.svg')] bg-repeat"></div>
        
        
        <div class="absolute inset-0 bg-gradient-to-br from-white/5 to-transparent opacity-0 transition-opacity duration-500 group-hover:opacity-100 pointer-events-none"></div>

        {@render children()}
    </a>
{:else}
    <div 
        class={cn(
            "relative overflow-hidden rounded-xl bg-[#141414] border border-white/5",
            className
        )}
        {style}
    >
        
        <div class="absolute inset-0 opacity-[0.03] pointer-events-none bg-[url('https://grainy-gradients.vercel.app/noise.svg')] bg-repeat"></div>
        
        {@render children()}
    </div>
{/if}
