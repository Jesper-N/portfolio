<script lang="ts" module>
	import { tv, type VariantProps } from "tailwind-variants";

	export const cardVariants = tv({
		base: "flex flex-col gap-6 rounded-xl",
		variants: {
			variant: {
				default: "bg-card text-card-foreground border py-6 shadow-sm",
				line: "bg-transparent border border-border/60 rounded-lg p-6 text-card-foreground",
			},
		},
		defaultVariants: {
			variant: "default",
		},
	});

	export type CardVariant = VariantProps<typeof cardVariants>["variant"];
</script>

<script lang="ts">
	import type { HTMLAttributes } from "svelte/elements";
	import { cn, type WithElementRef } from "$lib/utils.js";

	let {
		ref = $bindable(null),
		class: className,
		variant = "default",
		children,
		...restProps
	}: WithElementRef<HTMLAttributes<HTMLDivElement>> & { variant?: CardVariant } = $props();
</script>

<div
	bind:this={ref}
	data-slot="card"
	class={cn(cardVariants({ variant }), className)}
	{...restProps}
>
	{@render children?.()}
</div>
