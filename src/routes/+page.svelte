<script lang="ts">
	import {PUBLIC_BACKEND_URL} from "$env/static/public"
	import user from '../user';
	import Login from '$lib/login.svelte';
	import { goto } from '$app/navigation';
	import axios from 'axios';
	import { onMount } from 'svelte';
	import Leaderboard from '$lib/leaderboard.svelte';
	import { ProgressRadial } from '@skeletonlabs/skeleton';

	let leaderboard: LeaderboardUser[] = [];

	$: isLoggedIn = $user === null || Object.keys($user).length === 0 ? false : true;

	const logout = () => {
		user.update((val) => (val = null));
	};

	$: if (isLoggedIn) {
		goto('/dashboard');
	}

	const getLeaderboard = async () => {
		const res = await axios.get(`${PUBLIC_BACKEND_URL}/get-leaderboard`);
		leaderboard = await res.data;
	};

	onMount(getLeaderboard);
</script>

<div class="container h-full mx-auto flex justify-center items-center p-4">
	<div class="space-y-[6rem] flex flex-col">
		<h1 class="h1 mx-auto font-bold">Code Mason's Quiz</h1>
		<div class="">
			{#if !isLoggedIn}
				<Login />
			{:else}
				<div class="flex flex-row justify-center space-x-8">
					<button on:click={logout} class="btn variant-filled">Logout</button><button
						class="btn variant-filled">Dashboard</button
					>
				</div>
			{/if}
		</div>
		<div class="flex flex-col">
			<h1 class="h1 mx-auto font-bold">Leaderboard</h1>
			<div class="mt-[4rem]">
			{#if leaderboard.length > 0}
					<Leaderboard leaderboard={leaderboard} />
			{:else}
			<div class="flex flex-col justify-center items-center h-full">
				<ProgressRadial />
			</div>
			{/if}
		</div>
		</div>
	</div>
</div>
