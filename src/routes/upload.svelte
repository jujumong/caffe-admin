<script>
	import { uploadBytes, ref, getDownloadURL } from "firebase/storage";
	import { storage } from "../firebaseApp";

	let url;

	async function upload(event) {		
		if( !event.target.files ) {
			return;
		}
		const file = event.target.files[0];
		const fileRef = ref( storage, `uploads/${new Date().getTime()}.${file.name.split('.').pop()}` );
		await uploadBytes( fileRef, file );
		url = await getDownloadURL( fileRef );
	}
</script>

<input type="file" on:change={upload} />
{#if url}
	<p>{url}</p>
<!-- svelte-ignore a11y-missing-attribute -->
	<img src={url} />
{/if}