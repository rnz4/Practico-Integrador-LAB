 <script>
  import { slide } from "svelte/transition";
  import { elasticInOut } from "svelte/easing";
  
  
 
  let input="";
  let tareas = [];
  getTareas();
  
  async function getTareas(){
    const res = await fetch('http://127.0.0.1:5000/items/all', {
			method: 'GET'
		})		
		const json = await res.json()
		tareas = json.items
  }
  
  async function deleteTarea(aBorrar){
    let data = { item: aBorrar }
    const res = await fetch('http://127.0.0.1:5000/item/remove', {
      method: 'DELETE',
      headers: { 'Content-Type': 'application/json;charset=utf-8' },
      body: JSON.stringify(data)
		})	
		const json = await res.json()
		let result = JSON.stringify(json)
    getTareas()
  }
  async function newTarea(){
    let data = { item: input }
    const res = await fetch('http://127.0.0.1:5000/item/new', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json;charset=utf-8' },
      body: JSON.stringify(data)
		})
		
		const json = await res.json()
		let result = JSON.stringify(json)
    getTareas()
    
  }
</script>

<svelte:head>
  
    <link rel="stylesheet" href="https://jenil.github.io/bulmaswatch/default/bulmaswatch.min.css">
    <script src="https://use.fontawesome.com/releases/v5.3.1/js/all.js"></script>
</svelte:head>

<main class="container is-fluid">
  <body>  
  <div class="columns is-centered is-vcentered is-mobile">
    <div class="column is-narrow" style="width: 80%">
      <h1 class="has-text-centered title">TODO LIST Svelte</h1>
      <form class="field has-addons" style="justify-content: center" on:submit|preventDefault={newTarea}>
        <div class="control" style="width: 60%;">
          <input bind:value={input} class="input" type="text" placeholder="Nueva tarea">
        </div>
        <div class="control">
          <button class="button is-primary">
            <span class="icon is-small">
              <i class="fas fa-plus"></i>
            </span>
          </button>
        </div>
      </form>
      <ul class:list={tareas.length > 0}>
        {#each tareas as todo}
          <li class="list-item" transition:slide="{{duration: 300, easing: elasticInOut}}">
            <div class="is-flex" style="align-items: center">
              <span class="is-pulled-left">{todo[0]}</span>
              <div style="flex: 1"></div>
              <button class="button is-text is-pulled-right is-small" on:click={()=> deleteTarea(todo[0])}>
                <span class="icon">
                  <i class="fas fa-check"></i>
                </span>
              </button>
            </div>
          </li>
        {:else}
          <li class="has-text-centered" transition:slide="{{delay: 600, duration: 300, easing: elasticInOut}}">Nothing here!</li>
        {/each}
      </ul>
    </div>
  </div>
</main>


