<script>
  import { onAuthStateChanged } from "firebase/auth";

  import LoginRegister from "./lib/user-management/LoginRegister.svelte";
  import NewNoteForm from "./lib/NewNoteForm.svelte";
  import NotesList from "./lib/NotesList.svelte";
  import { auth } from "./firebase";
  import { buttonStyles } from "./style-vars";

  let loading = true
  let userLoggedIn = false
  let addNewNote = false

  let notesList

  onAuthStateChanged(auth, (user) => {
    if (user) {
      const uid = user.uid;
      loading = false
      userLoggedIn = true
    } else {
      loading = false
      userLoggedIn = false
    }
  });

  const hideForm = (e, type) => {
    e.preventDefault()
    addNewNote = false
    if (type === "add") {
      notesList.getNotes()
    }
  }

  const logout = () => {
    auth.signOut()
    userLoggedIn = false
  }

</script>
<main>
  <div class="bg-cyan-50 dark:bg-slate-900 app">
{#if loading}
    <div class="w-full">
      <button type="button" class={buttonStyles} disabled>
        <svg class="text-cyan-500 animate-spin inline" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg"
            width="24" height="24">
            <path
              d="M32 3C35.8083 3 39.5794 3.75011 43.0978 5.20749C46.6163 6.66488 49.8132 8.80101 52.5061 11.4939C55.199 14.1868 57.3351 17.3837 58.7925 20.9022C60.2499 24.4206 61 28.1917 61 32C61 35.8083 60.2499 39.5794 58.7925 43.0978C57.3351 46.6163 55.199 49.8132 52.5061 52.5061C49.8132 55.199 46.6163 57.3351 43.0978 58.7925C39.5794 60.2499 35.8083 61 32 61C28.1917 61 24.4206 60.2499 20.9022 58.7925C17.3837 57.3351 14.1868 55.199 11.4939 52.5061C8.801 49.8132 6.66487 46.6163 5.20749 43.0978C3.7501 39.5794 3 35.8083 3 32C3 28.1917 3.75011 24.4206 5.2075 20.9022C6.66489 17.3837 8.80101 14.1868 11.4939 11.4939C14.1868 8.80099 17.3838 6.66487 20.9022 5.20749C24.4206 3.7501 28.1917 3 32 3L32 3Z"
              stroke="currentColor" stroke-width="5" stroke-linecap="round" stroke-linejoin="round"></path>
            <path
              d="M32 3C36.5778 3 41.0906 4.08374 45.1692 6.16256C49.2477 8.24138 52.7762 11.2562 55.466 14.9605C58.1558 18.6647 59.9304 22.9531 60.6448 27.4748C61.3591 31.9965 60.9928 36.6232 59.5759 40.9762"
              stroke="currentColor" stroke-width="5" stroke-linecap="round" stroke-linejoin="round" class="text-gray-900">
            </path>
          </svg>
        &nbsp;Loading...
      </button>
    </div>
    
{:else}
    <div class="header bg-cyan-300 dark:bg-slate-950">
      <h1 class="text-5xl font-bold text-slate-900 dark:text-white text-center">📝Reyaly Notes App📝</h1>
      <h3 class="text-2xl font-bold text-slate-900 dark:text-white text-center">(Svelte Edition)</h3>
    </div>

    {#if userLoggedIn}
      <button class={buttonStyles} on:click={() => (addNewNote = !addNewNote)}>+ Add New</button>
      {#if addNewNote}
        <NewNoteForm hideForm={hideForm} />
      {/if}
      <NotesList bind:this={notesList} />
      <button class={buttonStyles} on:click={logout}>Logout</button>
    {:else}
      <LoginRegister />
    {/if}
{/if}
</div>
</main>

<style global>
  .app {
    min-height: 100vh;
    padding: 100px 0;
    font-family: Arial, Helvetica, sans-serif;
  }

  h1 {
    margin-bottom: 10px;
  }

  button {
    box-shadow: var(--box-shadow);
  }

  .header {
    padding: 50px;
    width: fit-content;
    margin: 0 auto;
    border-radius: 10px;
    box-shadow: var(--box-shadow);
  }

  .loader {
    border: 10px solid #f3f3f3; /* Light grey */
    border-top: 10px solid #3498db; /* Blue */
    border-radius: 50%;
    width: 50px;
    height: 50px;
    animation: spin 2s linear infinite;
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
</style>
