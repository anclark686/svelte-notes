<script>
import {
  collection,
  query,
  where,
  getDocs
} from "firebase/firestore";
import {
  onMount
} from "svelte";

import {
  auth,
  db,
} from "../firebase";
import NotesCard from "./NotesCard.svelte";
import NotePreview from "./NotePreview.svelte";
import SearchFilter from "./SearchFilter.svelte";

let notes = []
let note
let showNoteDetails = false
let tags = []
let filteredNotes = []

export const getNotes = async () => {
  notes = []
  const uid = auth.currentUser.uid
  const q = query(collection(db, "notes"), where("uid", "==", uid));

  const querySnapshot = await getDocs(q);
  querySnapshot.forEach((doc) => {
    notes = [...notes, doc.data()]
    filteredNotes = notes

    doc.data().tags.forEach((tag) => {
      if (!tags.includes(tag)) {
        tags = [...tags, tag]
      }
    })
  });
}

const searchNotes = (searchTerm) => {
  filteredNotes = []

  notes.forEach((note) => {
    const everything = `${note.title} ${note.content} ${note.tags.join(" ")}`.toLowerCase()

    if (everything.includes(searchTerm)) {
      filteredNotes = [...filteredNotes, note]
    }
  })
}

const setSearch = (e) => {
  const data = new FormData(e.target)
  const search = data.get("search")

  if (search) {
    searchNotes(search.toString().toLowerCase())
  }
}

const filterNotes = (filterTerm) => {
  if (filterTerm === "all") {
    filteredNotes = notes
    return
  }

  filteredNotes = []

  notes.forEach((note) => {
    if (note.tags.includes(filterTerm)) {
      filteredNotes = [...filteredNotes, note]
    }
  })
}

const setFilter = (filter) => {
  filterNotes(filter)
}

const showNote = (selectedNote) => {
  showNoteDetails = true
  note = selectedNote
}

const hideNote = () => {
  showNoteDetails = false
  note = {}
}

onMount(() => {
  getNotes()
})
</script>

<main>
    <SearchFilter tagArray={tags} setSearch={setSearch} setFilter={setFilter} />
    <div class="notes-container bg-cyan-100 dark:bg-slate-950 p-8 w-2/3 mx-auto my-10 rounded-md flex flex-wrap justify-center">
        {#each filteredNotes as note}
        <button on:click={() => showNote(note)} class="m-2">
            <NotesCard note={note} />
        </button>
        {/each}
    </div>

    {#if showNoteDetails}
      <div class="note-details-container w-2/3">
        <NotePreview note={note} hideNote={hideNote}/>
      </div>
    {/if}
</main>

<style>
.notes-container {
  box-shadow: var(--box-shadow);
}
</style>
