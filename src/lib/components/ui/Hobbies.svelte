<script lang="ts">
    import TactileCard from './TactileCard.svelte';
</script>

<TactileCard class="w-full p-8 md:p-12 relative overflow-hidden group">
    <!-- Background visual -->
    <div class="absolute inset-0 bg-[url('https://images.unsplash.com/photo-1547754980-3df97fed72a8?q=80&w=2670&auto=format&fit=crop')] bg-cover bg-center opacity-10 mix-blend-luminosity group-hover:opacity-20 transition-opacity duration-700"></div>
    <div class="absolute inset-0 bg-gradient-to-r from-black via-black/90 to-transparent"></div>

    <div class="relative z-10 flex flex-col md:flex-row gap-12 items-end justify-between">
        <div class="space-y-6 max-w-xl">
            <h3 class="font-mono text-sm uppercase tracking-widest text-primary">Offline / Hobbies</h3>
            <h2 class="text-3xl md:text-4xl font-medium text-white leading-tight">
                From the server room<br>to the <span class="text-muted-foreground italic">sim rig</span>.
            </h2>
            <div class="space-y-4 text-muted-foreground leading-relaxed">
                <p>
                    Former professional esports competitor (2 years) with a mindset built on optimization and reaction time.
                </p>
                <p>
                    Currently channeling that competitive drive into high-fidelity Sim Racing. Exploring the physics of virtual motorsport and the telemetry data that powers it.
                </p>
            </div>
        </div>

        <!-- Telemetry Viz Placeholder -->
        <div class="w-full md:w-64 space-y-3 font-mono text-xs">
            <div class="flex justify-between text-muted-foreground">
                <span>THROTTLE</span>
                <span>88%</span>
            </div>
            <div class="h-1 w-full bg-white/10 rounded-full overflow-hidden">
                <div class="h-full bg-primary w-[88%]"></div>
            </div>
            
            <div class="flex justify-between text-muted-foreground pt-2">
                <span>BRAKE</span>
                <span>12%</span>
            </div>
            <div class="h-1 w-full bg-white/10 rounded-full overflow-hidden">
                <div class="h-full bg-red-500 w-[12%]"></div>
            </div>

            <div class="flex justify-between text-muted-foreground pt-2">
                <span>APM</span>
                <span>342</span>
            </div>
            <div class="h-1 w-full bg-white/10 rounded-full overflow-hidden">
                <div class="h-full bg-blue-500 w-[75%]"></div>
            </div>
        </div>
    </div>
</TactileCard>