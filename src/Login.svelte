<script lang="ts" context="module">

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

export async function AuthenticateSafe(username:string, password:string) {
    throw new Error("Function not implemented");
}

export async function AuthenticatePasswd(endpoint:string, username:string, password:string, schoolUUID:string):Promise<Token>{
    const response = await fetch(endpoint, {
    	method: 'POST',
    	body: JSON.stringify({username, password, schoolUUID}),
    	headers: {'Content-Type': 'application/json', 'Accept': 'application/json'}
    }).catch((err) => {
        console.error('catch', err);
        throw Error(err)
    })
	//TODO better error handling
    if (!response.ok){
        throw Error(response.statusText)
    }

    const result = await response.json()
    return result
    
}

//utils
export function ParseForm(form):LoginForm {
	const formData = new FormData(form.target);
	//typescript thing
	let result = {
		username: undefined,
		password: undefined,
		save: undefined
	};
	for (const field of formData) {
		const [key, value] = field;
		result[key] = value;
	}

	return result;
}
</script>