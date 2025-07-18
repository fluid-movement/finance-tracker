<script lang="ts">
    import { authClient } from '$lib/auth-client';
    import Button from '$lib/components/ui/button/button.svelte';
    import { toast } from 'svelte-sonner';
</script>

<Button
	onclick={async () => {
		await authClient.signOut().then(() => {
			toast('Logged out');
		});
	}}
> 
    Sign Out
</Button>
