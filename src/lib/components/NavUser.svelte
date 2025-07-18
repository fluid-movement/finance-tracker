<script lang="ts">
	import { EllipsisVertical, LogOut, CircleUserRound } from '@lucide/svelte';
	import * as Avatar from '$lib/components/ui/avatar';
	import * as DropdownMenu from '$lib/components/ui/dropdown-menu/index';
	import * as Sidebar from '$lib/components/ui/sidebar';
	import { authClient } from '$lib/auth-client';
	import Button from '$lib/components/ui/button/button.svelte';

	const session = authClient.useSession();
	let userName = $state($session?.data?.user?.name ?? '');
	let userEmail = $state($session?.data?.user?.email ?? '');
	let userAvatar = $state($session?.data?.user?.image ?? '');

	let loggedIn = $state($session?.data ? true : false);

	const sidebar = Sidebar.useSidebar();
</script>

<Sidebar.Menu>
	<Sidebar.MenuItem>
		{#if loggedIn}
			<DropdownMenu.Root>
				<DropdownMenu.Trigger>
					{#snippet child({ props })}
						<Sidebar.MenuButton
							{...props}
							size="lg"
							class="data-[state=open]:bg-sidebar-accent data-[state=open]:text-sidebar-accent-foreground"
						>
							<Avatar.Root class="size-8 rounded-lg grayscale">
								<Avatar.Image src={userAvatar} alt={userName} />
								<Avatar.Fallback class="rounded-lg">CN</Avatar.Fallback>
							</Avatar.Root>
							<div class="grid flex-1 text-left text-sm leading-tight">
								<span class="truncate font-medium">{userName}</span>
								<span class="text-muted-foreground truncate text-xs">
									{userEmail}
								</span>
							</div>
							<EllipsisVertical />
						</Sidebar.MenuButton>
					{/snippet}
				</DropdownMenu.Trigger>
				<DropdownMenu.Content
					class="w-(--bits-dropdown-menu-anchor-width) min-w-56 rounded-lg"
					side={sidebar.isMobile ? 'bottom' : 'right'}
					align="end"
					sideOffset={4}
				>
					<DropdownMenu.Label class="p-0 font-normal">
						<div class="flex items-center gap-2 px-1 py-1.5 text-left text-sm">
							<Avatar.Root class="size-8 rounded-lg">
								<Avatar.Image src={userAvatar} alt={userName} />
								<Avatar.Fallback class="rounded-lg">CN</Avatar.Fallback>
							</Avatar.Root>
							<div class="grid flex-1 text-left text-sm leading-tight">
								<span class="truncate font-medium">{userName}</span>
								<span class="text-muted-foreground truncate text-xs">
									{userEmail}
								</span>
							</div>
						</div>
					</DropdownMenu.Label>
					<DropdownMenu.Separator />
					<DropdownMenu.Group>
						<DropdownMenu.Item>
							<CircleUserRound />
							Account
						</DropdownMenu.Item>
					</DropdownMenu.Group>
					<DropdownMenu.Separator />
					<DropdownMenu.Item onclick={async () => {
						await authClient.signOut();
					}}>
						<LogOut />
						Log out
					</DropdownMenu.Item>
				</DropdownMenu.Content>
			</DropdownMenu.Root>
		{:else}
			<Button href="/login" class="w-full">Sign In</Button>
		{/if}
	</Sidebar.MenuItem>
</Sidebar.Menu>
