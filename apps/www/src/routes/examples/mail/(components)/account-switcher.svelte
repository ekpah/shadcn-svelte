<script lang="ts">
	import * as Select from "@/registry/new-york/ui/select";
	import { cn } from "@/utils";
	import type { Account } from "../data.js";

	export let isCollapsed: boolean;
	export let accounts: Account[];

	let selectedAccount = accounts[0];
</script>

<Select.Root
	portal={null}
	selected={{ value: selectedAccount.email, label: selectedAccount.label }}
	onSelectedChange={(e) => {
		selectedAccount = accounts.find((account) => account.email === e?.value) || accounts[0];
	}}
>
	<Select.Trigger
		class={cn(
			"flex items-center gap-2 [&>span]:line-clamp-1 [&>span]:flex [&>span]:w-full [&>span]:items-center [&>span]:gap-1 [&>span]:truncate [&_svg]:h-4 [&_svg]:w-4 [&_svg]:shrink-0",
			isCollapsed &&
				"flex h-9 w-9 shrink-0 items-center justify-center p-0 [&>div>svg]:hidden [&>span]:w-auto"
		)}
		aria-label="Select account"
	>
		<span class="pointer-events-none">
			<svelte:component this={selectedAccount.icon} class={cn(isCollapsed ? "ml-2" : "")} />
			<span class={cn(isCollapsed ? "!ml-0 !hidden" : "ml-2")}>
				{selectedAccount.label}
			</span>
		</span>
	</Select.Trigger>
	<Select.Content sameWidth={!isCollapsed} align={isCollapsed ? "start" : undefined}>
		<Select.Group>
			{#each accounts as account}
				<Select.Item value={account.email} label={account.label}>
					<div
						class="flex items-center gap-3 [&_svg]:h-4 [&_svg]:w-4 [&_svg]:shrink-0 [&_svg]:text-foreground"
					>
						<svelte:component
							this={account.icon}
							aria-hidden="true"
							class="size-4 shrink-0 text-foreground"
						/>
						{account.email}
					</div>
				</Select.Item>
			{/each}
		</Select.Group>
	</Select.Content>
	<Select.Input hidden name="account" />
</Select.Root>
