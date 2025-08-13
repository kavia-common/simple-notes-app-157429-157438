<script lang="ts">
    export let note: { id: string, title: string, content: string, updated: number } | null = null;
    export let editMode = false;
    export let onUpdate: (id:string, newNoteData: {title:string,content:string}) => void;
    export let onDelete: (id:string) => void;
    export let onCancel: () => void;
    export let onEdit: () => void;

    let editedTitle = '';
    let editedContent = '';

    $: if (note && editMode) {
        editedTitle = note.title;
        editedContent = note.content;
    }
    function handleSave() {
        if (note && editedTitle.trim() !== '') {
            onUpdate?.(note.id, {
                title: editedTitle.trim(),
                content: editedContent
            });
        }
    }
</script>

{#if !note}
    <div class="empty-note">
        <p>Select or create a note</p>
    </div>
{:else if editMode}
    <div class="note-edit">
        <input
            class="note-title"
            bind:value={editedTitle}
            maxlength={128}
            required
            placeholder="Title"
        />
        <textarea
            class="note-content"
            bind:value={editedContent}
            rows={10}
            placeholder="Start typing your note..."
        ></textarea>
        <div class="actions">
            <button class="primary" on:click={handleSave} disabled={editedTitle.trim() === ''}>Save</button>
            <button class="secondary" on:click={onCancel}>Cancel</button>
            <button class="delete" on:click={() => onDelete(note.id) } >Delete</button>
        </div>
    </div>
{:else}
    <div class="note-view">
        <h2>{note.title || 'Untitled'}</h2>
        <div class="meta">Updated: {new Date(note.updated).toLocaleString()}</div>
        <pre class="note-content">{note.content}</pre>
        <div class="actions">
            <button class="primary" on:click={onEdit}>Edit</button>
            <button class="delete" on:click={() => onDelete(note.id)}>Delete</button>
        </div>
    </div>
{/if}

<style>
.empty-note {
    min-height: 50vh;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--color-text-secondary);
    font-size: 1.3rem;
    font-style: italic;
}
.note-edit, .note-view {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    background: #fff;
    border-radius: var(--radius);
    box-shadow: var(--shadow);
    padding: 2rem;
}
.note-title {
    font-size: 1.4rem;
    font-weight: 600;
    border: none;
    border-radius: var(--radius);
    background: var(--color-surface);
    padding: 0.6rem 0.8rem;
    margin-bottom: 0.3rem;
    color: var(--color-text-primary);
    border: 1px solid #e0e7ff;
}
.note-title:focus {
    outline: 2px solid var(--color-primary);
}
textarea.note-content {
    border: 1px solid #eee;
    border-radius: var(--radius);
    font-size: 1rem;
    padding: 0.8rem;
    background: var(--color-surface);
    color: var(--color-text-primary);
    min-height: 180px;
    resize: vertical;
}
.note-view h2 {
    margin-bottom: .1rem;
}
.note-view .meta { 
    color: var(--color-text-secondary);
    font-size: 0.9rem;
    margin-bottom: .9rem;
}
.note-view .note-content {
    white-space: pre-wrap;
    margin: 1rem 0;
    background: var(--color-surface);
    border-radius: var(--radius);
    padding: 1rem;
    overflow-x: auto;
}
.actions {
    display: flex;
    gap: 0.6rem;
    margin-top: 1rem;
}
button.primary {
    background: var(--color-primary);
    color: #fff;
    border: none;
    border-radius: var(--radius);
    padding: 0.5rem 1.2rem;
    font-size: 1.08rem;
    cursor: pointer;
    transition: background 0.15s;
}
button.primary:disabled {
    opacity: 0.6;
    cursor: not-allowed;
}
button.primary:hover {
    background: #3852db;
}
button.secondary {
    background: transparent;
    color: var(--color-secondary);
    border: 1px solid var(--color-secondary);
    border-radius: var(--radius);
    padding: 0.5rem 1.2rem;
    font-size: 1.08rem;
    cursor: pointer;
}
button.secondary:hover {
    background: #E9ECEF;
}
button.delete {
    background: var(--color-accent);
    color: #fff;
    border: none;
    border-radius: var(--radius);
    padding: 0.5rem 1.1rem;
    font-size: 1.08rem;
    cursor: pointer;
    transition: background 0.18s;
}
button.delete:hover {
    background: #ffd760;
    color: #232323;
}
</style>
