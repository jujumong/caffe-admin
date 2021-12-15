<script>
  import firebaseApp from "../firebaseApp";
  import { getFirestore, addDoc, collection, getDocs } from "firebase/firestore/lite";
	import { onMount } from "svelte";
	import Item from "../components/Item.svelte";

	let list = [];
  let name = '';
  let price = '';
  let errorBag = {};
  let touched = {};

	$: value = name + price; // useMemo

	// 반응형 구문
  $: {
    const newErrorBag = {};
    if (!name) {
      newErrorBag.name = '이름은 필수 입력 항목입니다.';
    }
    if (!price) {
      newErrorBag.price = '가격은 필수 입력 항목입니다.';
    }
		else if( isNaN(price) ) {
			newErrorBag.price = '가격은 숫자만 입력 가능합니다.';
		}
		else if( price < 0 ) {
			newErrorBag.price = '가격은 0보다 큰 숫자만 입력 가능합니다.'
		}
    errorBag = newErrorBag;
  } // useEffect

  const db = getFirestore(firebaseApp);
	const menuCollection = collection( db, 'menu' );

	async function load() {
		const docs = await getDocs(menuCollection);
		const newList = [];
		docs.forEach( doc => {
			newList.push({
				id: doc.id,
				name: doc.data().name,
				price: doc.data().price,
			});
		});
		list = newList;
	}

  function handleFocus(key) {
    touched = {
      ...touched,
      [key]: true,
    };
  }

  async function addItem() {
		if( Object.keys(errorBag).length > 0 ) {
			return;
		}
    await addDoc( menuCollection, {
      name,
      price: parseInt(price, 10),
    });
    name = '';
		price = '';
		errorBag = {};
		touched = {};
		load();
  }

	onMount(() => {
		load();
	});
</script>

<svelte:head>
  <title>Sapper project template</title>
</svelte:head>

<ul>
	{#each list as item (item.id)}
		<Item item={item} reload={load} />
	{/each}
</ul>

<form>
  <div>
    <input
      type="text"
      name="name"
      placeholder="name"
      on:focus={() => handleFocus("name")}
      bind:value={name}
    />
    {#if errorBag.name && touched.name}
      <div class="error">{errorBag.name}</div>
    {/if}
  </div>
  <div>
    <input
      type="text"
      name="price"
      placeholder="price"
      on:focus={() => handleFocus("price")}
      bind:value={price}
    />
    {#if errorBag.price && touched.price}
      <div class="error">{errorBag.price}</div>
    {/if}
  </div>
  <div>
    <button type="button" on:click={addItem}>추가</button>
  </div>
</form>

<style>
  input {
    width: 100%;
    padding: 4px;
    margin: 8px 0;
  }
  .error {
    font-size: 0.8em;
    color: red;
  }
</style>
