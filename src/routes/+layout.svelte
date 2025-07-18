<script lang="ts">
	import * as Sidebar from '$lib/components/ui/sidebar/index.js';
	import { Toaster } from '$lib/components/ui/sonner/index.js';
	import '../app.css';
	import { ModeWatcher } from 'mode-watcher';
	import SiteHeader from '$lib/components/SiteHeader.svelte';
	import AppSidebar from '$lib/components/AppSidebar.svelte';

	let { children } = $props();
</script>

<ModeWatcher />
<Toaster position="top-center" closeButton richColors />

<Sidebar.Provider>
	<AppSidebar />
	<Sidebar.Inset>
		<SiteHeader />
		<div class="flex flex-1 flex-col">
			<div class="@container/main flex flex-1 flex-col gap-2">
				<div class="flex flex-col gap-4 py-4 md:gap-6 md:py-6">
					{@render children()}
				</div>
			</div>
		</div>
	</Sidebar.Inset>
</Sidebar.Provider>
