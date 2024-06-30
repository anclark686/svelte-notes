<script>
  import { inputStyles, labelStyles, smallButtonStyles } from "../style-vars";

export let tagArray
export let setSearch
export let setFilter

let filter = "All"

const buttonStyles = smallButtonStyles.replace("p-2", "px-6 py-2")

const reset = () => {
  filter = "all"
  setFilter(filter)
  document.getElementById("search").value = ""
}
</script>

<div class="search-filter bg-cyan-100 dark:bg-slate-800 p-8 w-2/3 mx-auto my-10 rounded-md flex flex-wrap justify-center">
  <div class="flex w-full mx-auto text-center content-between ">
    <form action="submit" on:submit|preventDefault={setSearch} class="w-2/3">
      <div class="search-container w-full">
        <div class="sm:col-span-4 mb-2">
          <label for="search" class={labelStyles}>Search</label>
          <div class="mt-2 flex">
            <input id="search" name="search" type="search" class={inputStyles.replace("w-1/2 mx-auto", "w-3/4 mx-2")} />
            <input type="submit" value="Search" class={buttonStyles}/>
          </div>
        </div>
      </div>
    </form>
    
    <div class="filter-container grow">
      <div class="search-container w-full">
        <div class="sm:col-span-4 mb-2">
          <label for="tags" class={labelStyles}>Filter by Tag</label>
          <div class="mt-2 flex">
            <select name="tags" id="tags" class={inputStyles.replace("w-1/2 mx-auto", "w-3/4 mx-2")} on:change={(e) => filter = e.target.value}>
              <option value="all">All</option>
              {#each tagArray as tag}
                <option value={tag}>{tag}</option>
              {/each}
            </select>
            <input type="submit" value="Filter" class={buttonStyles} on:click={() => setFilter(filter)}/>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="btn-container w-full text-center">
    <button class={buttonStyles} on:click={reset}>Reset</button>
  </div>
</div>

<style>
  .search-filter {
    box-shadow: var(--box-shadow);
  }
  </style>
  