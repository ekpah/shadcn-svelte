<script lang="ts">
	import type { Command as CommandPrimitive } from "cmdk-sv";
	import * as Command from "@/registry/new-york/ui/command/index.js";
	import type { Model } from "../(data)/models";
	import Check from "svelte-radix/Check.svelte";
	import { cn } from "@/utils";

	type $$Props = {
		model: Model;
		isSelected: boolean;
		onSelect: () => void;
		onPeek: (model: Model) => void;
	} & CommandPrimitive.ItemProps;

	export let model: $$Props["model"];
	export let isSelected: $$Props["isSelected"];
	export let onSelect: $$Props["onSelect"];
	export let onPeek: $$Props["onPeek"];

	function mutationObserverAction(node: HTMLElement) {
		const observer = new MutationObserver((mutations) => {
			for (const mutation of mutations) {
				if (mutation.type !== "attributes" || mutation.attributeName !== "aria-selected")
					continue;

				if (node.getAttribute("aria-selected") === "true") {
					onPeek(model);
				}
			}
		});

		observer.observe(node, {
			attributes: true,
		});
		return {
			destroy() {
				observer.disconnect();
			},
		};
	}
</script>

<Command.Item value={model.name} asChild {onSelect} let:action let:attrs>
	<div
		{...attrs}
		use:mutationObserverAction
		use:action
		{...$$restProps}
		class="relative flex cursor-default select-none items-center rounded-sm px-2 py-1.5 text-sm outline-none aria-selected:bg-primary aria-selected:text-primary-foreground data-[disabled]:pointer-events-none data-[disabled]:opacity-50"
	>
		{model.name}
		{#if isSelected}
			<Check className={cn("ml-auto h-4 w-4")} />
		{/if}
	</div>
</Command.Item>
