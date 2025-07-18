<script lang="ts">
	import { authClient } from '$lib/auth-client';
	import Button from '$lib/components/ui/button/button.svelte';
	import * as Card from '$lib/components/ui/card';
	import * as Tabs from '$lib/components/ui/tabs/index.js';
	import Input from '$lib/components/ui/input/input.svelte';
	import { goto } from '$app/navigation';

	let name = $state('');
	let email = $state('');
	let password = $state('');

	let errorMessage = $state('');
	
	let tab = $state<'login' | 'register'>('login');

	const signIn = async () => {
		const { error } = await authClient.signIn.email({
			email: email,
			password: password
		});

		console.log(error);

		if (error?.message) {
			console.error('Login failed:', error);
			errorMessage = error.message || 'An error occurred during login.';
		} else {
			await goto('/');
		}
	};

	const signUp = async () => {
		const { error } = await authClient.signUp.email({
			name: name,
			email: email,
			password: password
		});

		if (error?.message) {
			console.error('Sign up failed:', error);
			errorMessage = error.message || 'An error occurred during sign up.';
		} else {
		    await goto('/');
		}
	};
</script>

<Card.Root class="mx-auto max-w-2xl">
	<Card.Content>
		<Tabs.Root value={tab}>
			<Tabs.List class="w-full">
				<Tabs.Trigger value="login">Log In</Tabs.Trigger>
				<Tabs.Trigger value="register">Sign Up</Tabs.Trigger>
			</Tabs.List>
			<Tabs.Content value="login">
				<Input type="text" placeholder="Email" bind:value={email} class="mb-2" />
				<Input type="password" placeholder="Password" bind:value={password} class="mb-2" />
				<Button onclick={signIn} class="w-full">Log In</Button>
			</Tabs.Content>
			<Tabs.Content value="register">
				<Input type="text" placeholder="Name" bind:value={name} class="mb-2" />
				<Input type="text" placeholder="Email" bind:value={email} class="mb-2" />
				<Input type="password" placeholder="Password" bind:value={password} class="mb-2" />
				<Button onclick={signUp} class="w-full">Sign Up</Button>
			</Tabs.Content>
		</Tabs.Root>
	</Card.Content>
	{#if errorMessage}
		<Card.Footer>
			<p class="text-red-500">{errorMessage}</p>
		</Card.Footer>
	{/if}
</Card.Root>
