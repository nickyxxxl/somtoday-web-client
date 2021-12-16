<script lang="ts" context="module">

interface loginForm {
    username:string,
    password:string,
    save:boolean
}

export async function AuthenticateSafe(username:string, password:string) {
    throw new Error("Function not implemented");
}

export async function AuthenticatePasswd(endpoint:string, username:string, password:string, schoolUUID:string):Promise<object>{
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
export function ParseForm(form):loginForm {
	const formData = new FormData(form.target);
	//typescript thing
	let result = {
		username: undefined,
		password: undefined,
		save: undefined
	};
	for (let field of formData) {
		const [key, value] = field;
		result[key] = value;
	}

	return result;
}
</script>