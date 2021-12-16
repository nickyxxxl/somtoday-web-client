<script lang="ts">
	import { AuthenticatePasswd, ParseForm } from "./Login.svelte";
	
	let curUUID = "b6256752-bbcf-42e0-8c7c-9e4643f0e827"
	let token: object
	let loggedIn: boolean = false;
	// const endpoint = 'https://nickyxxxl.nl/api/v1/auth'
	const endpoint = 'http://localhost:3000/auth'

	interface loginForm {
    	username:string,
    	password:string,
    	save:boolean
	}

	async function handleUnsafeLogin(target) {
		const form:loginForm = ParseForm(target)
		token = await AuthenticatePasswd(endpoint, form.username, form.password, curUUID)
		loggedIn = true;
		return
	}

</script>

<main>
{#if loggedIn == false}

<div class="center">
	<h2>Somtoday Login <b>(WIP)</b></h2>
	<form on:submit|preventDefault={handleUnsafeLogin}><fieldset>
		<label disabled for="school">School (locked)</label>
		<input disabled type="search" placeholder="Ausustinianum" name="schoolUUID" id="school">

		<label for="username">Username</label>
		<input type="text" placeholder="4403" name="username" id="username">

		<label for="password">Password</label>
		<input type="password" name="password" id="password">

		<label for="save">Remember login</label>
		<input type="checkbox" value="save" id="save"><br>

		<input class="button-outline" type="submit" value="Login">
	</fieldset></form>
</div>
{:else}

	<p>Logged in!</p>
	<p>Token: {JSON.stringify(token)}</p>

{/if}
</main>

<style>

.center{
	transform: translate(50%,50%);
	max-width: 50%;
	max-height: 50%;
}
input[disabled], label[disabled]{
	cursor: not-allowed;
	opacity: 50%;
}
</style>

<svelte:head>
	<link rel="stylesheet" href="milligram.css">
</svelte:head>