<script lang="ts">
    import { Toaster } from "$lib/components/ui/sonner/index.js";
	import {Separator} from '$lib/components/ui/separator';
	import '../app.css';
	import { authClient } from "$lib/auth-client";
	import Button from "$lib/components/ui/button/button.svelte";
	import LogOutButton from "$lib/components/LogOutButton.svelte";
	const session = authClient.useSession();
	let { children } = $props();
</script>

<Toaster position="top-center" closeButton richColors/>

<div class="max-w-7xl mx-auto px-4 py-8">
    <div class="flex items-center justify-between">
        <a href="/"><h1 class="text-3xl font-extrabold tracking-tight">Wow Amazing</h1></a>
        <div class="flex items-center gap-4">
            {#if $session.data}
                <span class="text-gray-500">Hi {$session.data.user.name}</span>
                <LogOutButton />
            {:else}
                <Button href="/login" variant="outline">Log In</Button>
            {/if}
        </div>
    </div>
    <Separator class="my-4" />
    {@render children()}
</div>
