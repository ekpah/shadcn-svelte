<script lang="ts">
	import { page } from "$app/stores";
	import { dev } from "$app/environment";
	import { Metadata, SiteFooter, SiteHeader, TailwindIndicator } from "$components/docs";
	import { updateTheme } from "@/utils";
	import "../styles/globals.css";
	import { config } from "@/stores";
	import { ModeWatcher } from "mode-watcher";
	import { Toaster as DefaultSonner } from "@/registry/default/ui/sonner";
	import { Toaster as NYSonner } from "@/registry/new-york/ui/sonner";

	$: updateTheme($config.theme, $page.url.pathname);
</script>

<ModeWatcher />
<Metadata />
{#if $config.style === "new-york"}
	<NYSonner />
{:else}
	<DefaultSonner />
{/if}

<div class="relative flex min-h-screen flex-col bg-background" id="page" data-vaul-drawer-wrapper>
	<SiteHeader />
	<div class="flex-1">
		<slot />
	</div>
	<SiteFooter />
	{#if dev}
		<TailwindIndicator />
	{/if}
</div>
