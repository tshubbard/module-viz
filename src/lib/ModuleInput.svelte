<script lang="js">
  import { moduleDefStore } from '@stores'

  let showErrorMsg = false
  let text = ''
  let hasModuleDef = false

  const onModuleChange = () => {
    let moduleDef = ''
    try {
      moduleDef = JSON.parse(text)
      showErrorMsg = false
      moduleDefStore.set(moduleDef)
    } catch(err) {
      showErrorMsg = true
      console.log('err: ', err)
    }
  }
</script>

<div>
  <h3>Paste Module Definition</h3>
  <textarea rows="4"
    bind:value={text}
    on:input={onModuleChange}
  />

  {#if showErrorMsg}
    <p class="text-red-600 font-bold">Incorrect JSON data!</p>
  {/if}
</div>

<style>
  textarea {
    width: 32rem;
  }  
</style>