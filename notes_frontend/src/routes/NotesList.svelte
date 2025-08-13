<script lang="ts">
    export let notes: { id: string, title: string, content: string, updated: number }[] = [];
    export let selectedNoteId: string | null = null;
    export let search = '';
    export let onSelect: (id: string) => void;
    export let onAdd: () => void;
    export let onSearch: (value:string) => void;

    function handleSearch(event: Event) {
        onSearch?.((event.target as HTMLInputElement).value);
    }
</script>

<div class="sidebar-search">
    <input
        type="text"
        placeholder="Search notes"
        value={search}
        on:input={handleSearch}
        aria-label="Search notes" />
    <button class="accent" title="New note" on:click={onAdd}>+</button>
</div>
<ul class="notes-list">
    {#if notes.length === 0}
        <li class="empty-state">No notes found</li>
    {/if}
    {#each notes as note (note.id)}
    <li>
        <button
            type="button"
            class:selected={note.id === selectedNoteId}
            on:click={() => onSelect(note.id)}
            aria-current={note.id === selectedNoteId ? "true" : undefined}
        >
            <strong>{note.title || 'Untitled'}</strong>
            <div class="updated">{new Date(note.updated).toLocaleString()}</div>
        </button>
    </li>
    {/each}
</ul>

<style>
.sidebar-search {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0 var(--padding) 0.5rem var(--padding);
}
.sidebar-search input {
    flex: 1;
    padding: 0.4rem 0.7rem;
    border-radius: var(--radius);
    border: 1px solid #ddd;
    font-size: 1rem;
    background: var(--color-surface);
    color: var(--color-text-primary);
}
.sidebar-search input:focus {
    outline: 2px solid var(--color-primary);
    border-color: var(--color-primary);
}
.sidebar-search button.accent {
    background: var(--color-accent);
    color: #fff;
    border: none;
    border-radius: var(--radius);
    font-size: 1.4rem;
    line-height: 1;
    width: 2.2rem;
    height: 2.2rem;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
}
.sidebar-search button.accent:hover {
    box-shadow: 0 2px 8px #ffd76045;
    background: #ffc84f;
}

ul.notes-list {
    list-style: none;
    margin: 0;
    padding: 0 var(--padding);
    overflow-y: auto;
    flex: 1;
}
ul.notes-list li {
    padding: 0;
    margin: 0 0 0.4rem 0;
    border-radius: var(--radius);
    list-style: none;
}
ul.notes-list li button {
    padding: .6rem .5rem;
    border-radius: var(--radius);
    width: 100%;
    text-align: left;
    background: transparent;
    border: 1px solid transparent;
    display: flex;
    flex-direction: column;
    gap: 0.1em;
    cursor: pointer;
    transition: background 0.15s, color 0.15s, border-color 0.15s;
    font-size: 1rem;
    color: var(--color-text-primary);
}
ul.notes-list li button.selected,
ul.notes-list li button:hover {
    background: var(--color-primary);
    color: #fff;
    border: 1px solid var(--color-primary);
}
ul.notes-list li button.selected strong,
ul.notes-list li button.selected .updated {
    color: #fff;
}
ul.notes-list .updated {
    font-size: 0.80rem;
    color: var(--color-text-secondary);
}
.empty-state {
    color: var(--color-text-secondary);
    margin-top: 2rem;
    text-align: center;
    user-select: none;
    font-style: italic;
}
</style>
