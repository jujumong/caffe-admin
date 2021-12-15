<script>
	import firebaseApp from "../firebaseApp";
  import { getFirestore, doc, updateDoc, deleteDoc } from "firebase/firestore/lite";

	export let item;
	export let reload;

	let { id, name, price } = item;

	const db = getFirestore(firebaseApp);
	const docRef = doc( db, 'menu', id);

	async function update() {
		await updateDoc(docRef, { name, price });
		reload();
	}

	async function remove() {
		await deleteDoc(docRef);
		reload();
	}

</script>

<li>
	<div><input type="text" bind:value={name}></div>
	<div><input type="text" bind:value={price}></div>
	<div><button on:click={update}>수정</button></div>
	<div><button on:click={remove}>삭제</button></div>
</li>

<style>
	li {
		display: flex;
		flex-direction: row;
	}
</style>