<script>
import {
  smallButtonStyles
} from "../style-vars";

export let note
export let hideNote

const mainStyle = "w-2/3 bg-slate-200 dark:bg-slate-800 px-10 pb-10 mx-auto my-10 rounded-md fixed inset-0 z-10 overflow-y-auto h-fit"
</script>

<div class={`note-preview ${mainStyle}`}>
  <button class="w-full text-right mt-5" on:click={hideNote}>❌</button>
    <h2 class="text-3xl text-center my-5 text-slate-900 dark:text-white">{note.title}</h2>
    <hr />
    <p class="text-md text-center my-4 text-slate-900 dark:text-white">{note.content}</p>
    <hr />
    <p class="text-xl text-center mt-4 text-slate-900 dark:text-white">Tags</p>
    <div class="tag-container w-full text-center my-4">
        {#each note.tags as tag}
        <p class={smallButtonStyles}>
            {tag}
        </p>
        {/each}
    </div>
</div>

<style>
.note-preview {
  box-shadow: var(--box-shadow);
}
</style>
