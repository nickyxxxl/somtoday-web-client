<script lang="ts">
	import { AuthenticatePasswd, ParseForm } from "./Login.svelte";

	interface LoginForm {
    	username:string,
    	password:string,
    	save:boolean
	}

	interface Token {
    	access_token: string,
    	refresh_token: string,
    	api_url: string,
    	expires_in: number
	}
	
	let curUUID = "b6256752-bbcf-42e0-8c7c-9e4643f0e827"
	let token: Token | void 
	let loggedIn: boolean = false
	let errored = false
	const endpoint = 'https://nickyxxxl.nl/api/som/v1/auth'
	const redirect = 'http://nickyxxxl.nl/som/main'

	async function handleUnsafeLogin(target) {
		const form:LoginForm = ParseForm(target)
		token = await AuthenticatePasswd(endpoint, form.username, form.password, curUUID).catch(() => { errored = true })
		if (typeof token != 'undefined' && typeof token.access_token == 'undefined') {
			errored = true
		} else {
			Object.keys(token).forEach(key => {
				window.localStorage.setItem(key, token[key])
			})
			loggedIn = true
		}
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
{:else if !errored}

	<p>Logged in!</p>
	<p>Redirecting...</p>
	{window.location.href = redirect}

{/if}
{#if errored}
	<h1 class="error">ERROR!!!</h1>
	<h4 class="error">Shit's broken (see developer console for details)</h4>
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
.error{
	color: red;
	font-weight: bolder;
}
</style>

<svelte:head>
	<link rel="stylesheet" href="milligram.css">
</svelte:head>