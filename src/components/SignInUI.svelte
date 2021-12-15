<script>
	import { getAuth, signInWithEmailAndPassword } from 'firebase/auth';
	import firebaseApp from '../firebaseApp';

	let email = '';
	let password = '';

	const auth = getAuth(firebaseApp);

	async function submit(event) {
		event.preventDefault();
		try {
			await signInWithEmailAndPassword(auth, email, password);
		}
		catch {
			alert('로그인 실패');
		}
	}
</script>

<form on:submit={submit}>
	<div><input type="text" bind:value={email}></div>
	<div><input type="password" bind:value={password}></div>
	<div><button on:click={submit}>로그인</button></div>
</form>