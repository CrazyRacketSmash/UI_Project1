<script>
  import { onMount } from "svelte";
  import entriesData from "../src/data/mockHistory.json";

  export let entries = [];
  
  let editingId = null;
  let editedCaption = "";

  function startEdit(entry) {
    editingId = entry.id;
    editedCaption = entry.caption;
  }

  function saveEdit(id) {
    const entry = entries.find(e => e.id === id);
    if (entry) {
      entry.caption = editedCaption;
      editingId = null;
      alert("Entry updated!");
    }
  }
</script>

<div class="entries">
  {#each entries as entry}
    <div class="entry-card">
      <h3>{entry.date}</h3>
      <p><strong>Mood:</strong> {entry.mood}</p>
      <img src={entry.image} alt="entry image" width="200" />
      {#if editingId === entry.id}
        <textarea bind:value={editedCaption}></textarea>
        <button on:click={() => saveEdit(entry.id)}>💾 Save</button>
      {:else}
        <p>{entry.caption}</p>
        <button on:click={() => startEdit(entry)}>✏️ Edit</button>
      {/if}
      <p><strong>Gym:</strong> {entry.gym.didGym ? `Yes, ${entry.gym.duration} min` : "No"}</p>
    </div>
  {/each}
</div>

<style>
  .entries {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
    margin-top: 1rem;
  }
  .entry-card {
    padding: 1rem;
    border: 1px solid #ddd;
    border-radius: 12px;
    background: #fafafa;
  }
  textarea {
    width: 100%;
    height: 60px;
    margin-bottom: 0.5rem;
  }
  button {
    margin-right: 0.5rem;
  }
</style>
