<script lang="ts">
	import type { SvelteComponent } from "svelte";
	import type { PageData } from "./$types";
	import ChevronRight from "lucide-svelte/icons/chevron-right";
	import { page } from "$app/stores";
	import { DocsPager, TableOfContents } from "$components/docs";
	import { cn } from "$lib/utils";

	export let data: PageData;
	// eslint-disable-next-line no-undef, @typescript-eslint/no-explicit-any
	type Component = $$Generic<typeof SvelteComponent<any, any, any>>;
	$: component = data.component as unknown as Component;
	$: doc = data.metadata;
</script>

<main class="relative py-6 lg:gap-10 lg:py-8 xl:grid xl:grid-cols-[1fr_300px]">
	<div class="mx-auto w-full min-w-0">
		<div class="mb-4 flex items-center space-x-1 text-sm text-muted-foreground">
			<div class="overflow-hidden text-ellipsis whitespace-nowrap">Docs</div>
			<ChevronRight class="h-4 w-4" />
			<div class="font-medium text-foreground">{doc.title}</div>
		</div>
		<div class="space-y-2">
			<h1 class={cn("scroll-m-20 text-4xl font-bold tracking-tight")}>
				{doc.title}
			</h1>
			{#if doc.description}
				<p class="text-balance text-lg text-muted-foreground">
					{doc.description}
				</p>
			{/if}
		</div>

		<div class="markdown pb-12 pt-8" id="markdown">
			<svelte:component this={component} />
		</div>

		<DocsPager />
	</div>
	<div class="hidden text-sm xl:block">
		<div class="sticky top-16 -mt-10 h-[calc(100vh-3.5rem)] overflow-hidden pt-6">
			{#key $page.url.pathname}
				<TableOfContents />
			{/key}
		</div>
	</div>
</main>
