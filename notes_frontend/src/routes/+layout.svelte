<script lang="ts">
    import '../app.css';
</script>

<div class="app-shell">
    <header>
        <h1>Notes</h1>
    </header>
    <div class="content-shell">
        <aside>
            <slot name="sidebar"></slot>
        </aside>
        <main>
            <slot />
        </main>
    </div>
</div>

<style>
.app-shell {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    background: var(--color-background);
}
header {
    height: 60px;
    box-shadow: var(--shadow);
    background: #fff;
    display: flex;
    align-items: center;
    padding: 0 var(--padding);
    z-index: 2;
}
header h1 {
    font-size: 1.5rem;
    color: var(--color-primary);
    letter-spacing: 1px;
    font-weight: 700;
}
.content-shell {
    flex: 1;
    min-height: 0;
    display: flex;
    max-height: calc(100vh - 60px);
    background: var(--color-surface);
}
aside {
    width: 320px;
    min-width: 200px;
    background: #fff;
    box-shadow: 1px 0 0 #E9ECEF;
    display: flex;
    flex-direction: column;
    border-right: 1px solid #E9ECEF;
    padding: var(--padding) 0;
}
main {
    flex: 1;
    padding: var(--padding);
    overflow-y: auto;
}
@media (max-width: 700px) {
    .content-shell {
        flex-direction: column;
    }
    aside {
        width: 100%;
        min-width: 0;
        border-right: none;
        border-bottom: 1px solid #E9ECEF;
    }
    main {
        padding: var(--padding) 0.5rem;
    }
}
</style>
