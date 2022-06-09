<script lang="js">
	import { moduleDefStore } from '@stores'

  let moduleDef = {}
  let outputData = ''
  let sizeKb = 0
  let sizeMb = 0
  let counts = {
    xxtotal: {
      name: 'Total',
      count: 0
    }
  }

  const onParseModuleDef = (def) => {
    moduleDef = def
    const size = JSON.stringify(moduleDef).length
    sizeKb = (size / 1024).toFixed(2)
    sizeMb = (size / 1024000).toFixed(2)
    parseComponents(def.components || def)
  }

  const parseComponents = (comp) => {
    outputData += '<ul>'
    Object.keys(comp).forEach(key => {
      outputData += parseComponent(comp[key])
      if (comp[key].components?.length) {
        parseComponents(comp[key].components)
      }
    })
    outputData += '</ul>'
  }

  const parseComponent = (comp) => {
    const { key, type } = comp
    if (counts[type]) {
      counts[type].count++
    } else {
      counts[type] = {
        name: type,
        count: 1
      }
    }
    counts.xxtotal.count++

    return `<li>[${type}] - ${key}</li>`
  }

  moduleDefStore.subscribe(value => onParseModuleDef(value))
</script>

<div>
  <h1>Module Data</h1>
  <div>
    <h3>Module Size</h3>
    <div>
      <ul>
        <li>in Kb: {sizeKb}</li>
        <li>in Mb: {sizeMb}</li>
      </ul>
    </div>
  </div>
  <div>
    <h3>Component Counts</h3>
    <ul>
      {#each Object.entries(counts) as count}
        
          {#if count[1].name === 'Total'}
            <li><strong> {count[1].name}: {count[1].count} </strong></li><li>- - - - - -</li>
          {:else}
            <li>{count[1].name}: {count[1].count}</li>
          {/if}
        
      {/each}
    </ul>
  </div>
  <div>
    <h3>Component Structure</h3>

    {@html outputData}
  </div>
</div>