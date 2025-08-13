<script lang="ts">
    import { onMount } from 'svelte';
    import NotesList from './NotesList.svelte';
    import NoteDetail from './NoteDetail.svelte';

    interface Note {
        id: string
        title: string
        content: string
        updated: number
    }

    let notes: Note[] = [];
    let selectedNoteId: string | null = null;
    let search = '';
    let editMode = false;

    // Storage key for local persistence
    const STORAGE_KEY = 'simple_notes_app_storage_v1';

    function saveNotes() {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(notes));
    }
    function loadNotes() {
        try {
            const data = localStorage.getItem(STORAGE_KEY);
            return data ? JSON.parse(data) : [];
        } catch {
            return [];
        }
    }

    onMount(() => {
        notes = loadNotes();
        if (notes.length) {
            selectedNoteId = notes[0].id;
        }
    });

    $: if (selectedNoteId && !notes.some(n => n.id === selectedNoteId)) {
        selectedNoteId = notes.length ? notes[0].id : null;
    }
    $: selectedNote = selectedNoteId
        ? notes.find(n => n.id === selectedNoteId) || null
        : null;

    function handleSelect(id:string) {
        selectedNoteId = id;
        editMode = false;
    }
    function handleAdd() {
        // Create a blank untitled note, select it & start editing
        const id = Math.random().toString(36).slice(2) + Date.now();
        const newNote: Note = {
            id,
            title: '',
            content: '',
            updated: Date.now()
        };
        notes = [{...newNote}, ...notes];
        selectedNoteId = id;
        editMode = true;
        saveNotes();
    }
    function handleEdit() {
        editMode = true;
    }
    function handleCancel() {
        // If it's a new note & cancelled with blank title, remove
        if (selectedNote && !selectedNote.title && !selectedNote.content) {
            notes = notes.filter(n => n.id !== selectedNote.id);
            selectedNoteId = notes.length ? notes[0].id : null;
        }
        editMode = false;
        saveNotes();
    }
    function handleUpdate(id:string, data:{title:string, content:string}) {
        notes = notes.map(n => n.id === id
            ? { ...n, ...data, updated: Date.now() }
            : n
        );
        editMode = false;
        saveNotes();
    }
    function handleDelete(id:string) {
        if (window.confirm("Delete this note? This cannot be undone.")) {
            notes = notes.filter(n => n.id !== id);
            if (selectedNoteId === id) {
                selectedNoteId = notes.length ? notes[0].id : null;
                editMode = false;
            }
            saveNotes();
        }
    }
    function handleSearch(value:string) {
        search = value;
    }

    let selectedNote: Note | null = null;
</script>

<NotesList slot="sidebar"
    {notes}
    {selectedNoteId}
    {search}
    onSelect={handleSelect}
    onAdd={handleAdd}
    onSearch={handleSearch}
/>

<NoteDetail
    note={selectedNote}
    {editMode}
    onUpdate={handleUpdate}
    onDelete={handleDelete}
    onCancel={handleCancel}
    onEdit={handleEdit}
/>

<style>
:global(body) {
    background: var(--color-background);
}
</style>
