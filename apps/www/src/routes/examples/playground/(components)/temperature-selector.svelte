<script lang="ts">
	import type { Slider as SliderPrimitive } from "bits-ui";
	import * as HoverCard from "@/registry/new-york/ui/hover-card";
	import { Label } from "@/registry/new-york/ui/label";
	import { Slider } from "@/registry/new-york/ui/slider";

	export let value: SliderPrimitive.Props["value"];
</script>

<div class="grid gap-2 pt-2">
	<HoverCard.Root openDelay={200} closeDelay={100}>
		<HoverCard.Trigger asChild let:builder>
			<div class="grid gap-4" use:builder.action {...builder}>
				<div class="flex items-center justify-between">
					<Label for="temperature">Temperature</Label>
					<span
						class="w-12 rounded-md border border-transparent px-2 py-0.5 text-right text-sm text-muted-foreground hover:border-border"
					>
						{value}
					</span>
				</div>
				<Slider
					id="temperature"
					max={1}
					bind:value
					step={0.1}
					class="[&_[role=slider]]:h-4 [&_[role=slider]]:w-4"
					aria-label="Temperature"
				/>
			</div>
		</HoverCard.Trigger>
		<HoverCard.Content class="w-[260px] text-sm" align="start" side="left">
			Controls randomness: lowering results in less random completions. As the temperature
			approaches zero, the model will become deterministic and repetitive.
		</HoverCard.Content>
	</HoverCard.Root>
</div>
