<script lang="ts">
	import {PUBLIC_BACKEND_URL} from "$env/static/public"
	import axios from 'axios';
	import user from '../user';
	let username = '';
	let currentError = null;


	const login = async () => {
		const usernameObject = {
			username: username
		};

		try {
			const res = await axios.post(`${PUBLIC_BACKEND_URL}/get-validation`, usernameObject, {
				headers: {
					'Content-Type': 'application/json'
				}
			});
            user.update(val => val = {...res.data})
		} catch (error) {
			console.error(error);
            currentError = error
		}
	};
</script>

<form on:submit|preventDefault={login} class="flex flex-row space-x-4">
	<input class="input" type="text" id="username" bind:value={username} />
	<button class="btn variant-filled" type="submit">Login</button>
</form>
