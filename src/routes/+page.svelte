<script lang="ts">
	import { authClient } from '$lib/auth-client';
	const session = authClient.useSession();
</script>

<div>
	{#if $session.data}
		<div>
			<p>
				Hey {$session?.data?.user.name}
			</p>
		</div>
	{/if}
</div>
