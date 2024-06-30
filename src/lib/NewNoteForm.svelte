<script>
import {
  collection,
  addDoc
} from "firebase/firestore";

import {
  auth,
  db,
} from "../firebase";
import {
  buttonStyles,
  inputStyles,
  labelStyles,
  smallButtonStyles
} from "../style-vars";

export let hideForm
let tag = ""
let tags = []

const addTag = (e) => {
  e.preventDefault()

  tags = [...tags, tag]
  tag = ""
  document.getElementById("tag").value = ""
}

const removeTag = (e) => {
  e.preventDefault()

  tags = tags.filter((val) => val !== e.target.value)
}

const addNewNote = async (e) => {
  const data = new FormData(e.target);
  const title = data.get("title")
  const content = data.get("content")

  const uid = auth.currentUser.uid

  const docRef = await addDoc(collection(db, "notes"), {
    title: title,
    content: content,
    tags: tags,
    uid: uid
  });

  hideForm(e, "add")
}
</script>

<div class="fixed inset-0 z-10 overflow-y-auto">
  
    <form action="submit" class="new-note-form bg-slate-200 dark:bg-slate-800" on:submit|preventDefault={addNewNote}>
      <button class="w-full text-right" on:click={(e) => hideForm(e, "close")}>❌</button>
        <h2 class="text-3xl text-center text-slate-900 dark:text-white">Add a New Note</h2>

        <div class="sm:col-span-4 my-6">
            <label for="title" class={labelStyles}>Title</label>
            <div class="mt-2">
                <input id="title" name="title" type="text" class={inputStyles} />
            </div>
        </div>

        <div class="col-span-full">
            <label for="content" class={labelStyles}>Note Content</label>
            <div class="mt-2">
                <textarea id="content" name="content" rows="3" class={inputStyles.replace("w-1/2", "w-3/4")}></textarea>
            </div>
        </div>

        <div class="sm:col-span-4 my-6">

            <label for="email" class={labelStyles}>Tags</label>
            <div class="tag-container w-full text-center my-2">
                {#each tags as tag}
                <button class={smallButtonStyles}>
                    <button id={tag} value={tag} class="text-xs" on:click={removeTag}>❌&nbsp;</button>
                    {tag}
                </button>
                {/each}
            </div>
            <div class="mt-2 text-center">

                <div class="input-row inline-flex mx-auto w-3/4">
                    <input id="tag" name="tag" type="text" class={inputStyles.replace("w-1/2", "w-3/4")} on:change={(e) => tag = e.target.value}/>
                    <button class={smallButtonStyles} on:click={addTag}>Add Tag</button>
                </div>
            </div>
        </div>
        <input type="submit" value="Add Note" class={buttonStyles} />
    </form>
</div>

  
<style>
.new-note-form {
  margin: 100px auto;
  width: 50%;
  padding: 35px 35px 15px 35px;
  border-radius: 10px;
  box-shadow: var(--box-shadow);
}
</style>
