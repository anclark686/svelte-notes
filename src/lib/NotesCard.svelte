<script>
export let note

const mainStyle = "w-60 flex items-center justify-center rounded-md bg-cyan-300 dark:bg-cyan-800 text-slate-900 dark:text-white px-2 h-32"
</script>

<div class={`note-content ${mainStyle}`}>
    <h1 class="inline-block text-xl ">{note.title}</h1>
</div>

<style>
.note-content {
  box-shadow: var(--box-shadow);
}
</style>
