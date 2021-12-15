<script>
	import { onMount } from 'svelte';
	import { getAuth, onAuthStateChanged } from 'firebase/auth';
	import firebaseApp from "../firebaseApp";
	import Nav from '../components/Nav.svelte';
	import SignInUI from "../components/SignInUI.svelte";

	const auth = getAuth(firebaseApp);
	let user;

	export let segment;

	onMount(() => {
		onAuthStateChanged( auth, credential => {
			user = credential;
		})
	})
</script>

<style>
	main {
		position: relative;
		max-width: 56em;
		background-color: white;
		padding: 2em;
		margin: 0 auto;
		box-sizing: border-box;
	}
</style>

<Nav {segment}/>

<main>
	{#if user}
		<slot></slot>
	{:else}
		<SignInUI />
	{/if}
</main>