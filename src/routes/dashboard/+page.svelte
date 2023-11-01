<script lang="ts">
	import { goto } from '$app/navigation';
	import { onMount } from 'svelte';
	import user from '../../user';
	import { ProgressRadial } from '@skeletonlabs/skeleton';
	import axios from 'axios';
	import { PUBLIC_BACKEND_URL } from '$env/static/public';
	import PastResults from '$lib/past-results.svelte';

	let userDetails: UserDetails;
	let pastResults: PastResult[] = [];

	const logout = () => {
		user.update((val) => (val = null));
		goto('/');
	};

	$: isLoggedIn = $user === null || Object.keys($user).length === 0 ? false : true;

	onMount(() => {
		if (!isLoggedIn) {
			goto('/');
		} else {
			$user && (userDetails = $user);
			userDetails && getPastResults(userDetails.id);
		}
	});

	const getPastResults = async (userId: number) => {
		const userData = {
			userId: userId
		};
		const res = await axios.post(`${PUBLIC_BACKEND_URL}/get-past-results`, userData);
		pastResults = res.data;
	};
</script>

<div class="flex flex-col items-center h-[100vh]">
	{#if isLoggedIn}
		<div class="space-y-[4rem]">
			<h1 class="h1 mt-[2rem] font-bold">Dashboard</h1>
			<div class="space-x-8 mt-[4rem]">
				<button on:click={() => goto('/quiz')} class="btn variant-filled">Start Quiz</button>
				<button on:click={logout} class="btn variant-filled">Logout</button>
			</div>
		</div>
		<div class="mt-[4rem]">
			{#if pastResults.length > 0}
				<PastResults {pastResults} />
			{:else}
				<div class="flex flex-col justify-center items-center h-full">
					<h3>No past results found...</h3>
				</div>
			{/if}
		</div>
	{:else}
		<div class="flex flex-col justify-center items-center h-full">
			<ProgressRadial />
		</div>
	{/if}
</div>
