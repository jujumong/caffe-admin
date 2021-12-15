<script>
	import { onMount } from "svelte";
  import firebaseApp from "../firebaseApp";
  import { getFirestore, doc, setDoc, getDoc } from "firebase/firestore/lite";

	let content = '';

	const db = getFirestore(firebaseApp);
	const docRef = doc( db, 'site-meta', 'about-content' );

	async function save() {
		setDoc(docRef, { content });
		alert( '저장되었습니다' );
	}

	onMount(async() => {
		const result = await getDoc(docRef);
		if( result ) {
			content = result.data().content;
		}
	})
</script>

<textarea bind:value={content}></textarea>	
<button on:click={save}>저장</button>

<style>
	textarea {
		width: 100%;
		min-height: 30vh;	
		padding: 4px;
	}
</style>