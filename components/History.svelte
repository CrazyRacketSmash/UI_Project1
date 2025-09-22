<script>
  import { onMount } from "svelte";
  import entriesData from "../src/data/mockHistory.json";

  export let entries = [];
  
  let editingId = null;
  let editedCaption = "";

  function startEdit(entry) {
    editingId = entry.id;
    editedCaption = entry.caption || "";
  }

  function saveEdit(id) {
    const entry = entries.find(e => e.id === id);
    if (entry) {
      entry.caption = editedCaption;
      editingId = null;
      alert("Entry updated!");
    }
  }

  function formatDuration(duration) {
    return duration && duration > 0 ? `${duration} min` : "Not recorded";
  }

  function formatSleep(sleep) {
    if (!sleep) return "Not recorded";
    return `${sleep.hours || 0}h ${sleep.quality ? "(Good quality)" : "(Poor quality)"}`;
  }

  function formatWater(water) {
    if (!water) return "Not recorded";
    return `${water.cups || 0} cups ${water.metGoal ? "(Goal met)" : "(Goal not met)"}`;
  }
</script>

<div class="entries">
  {#each entries as entry}
    <div class="entry-card">
      <div class="entry-header">
        <h3>📅 {entry.date}</h3>
      </div>
      
      <div class="activities-grid">
        <!-- Sleep -->
        {#if entry.sleep}
          <div class="activity-item">
            <span class="activity-icon">🛌</span>
            <div class="activity-content">
              <strong>Sleep:</strong> {formatSleep(entry.sleep)}
            </div>
          </div>
        {/if}

        <!-- Mood -->
        {#if entry.mood}
          <div class="activity-item">
            <span class="activity-icon">😊</span>
            <div class="activity-content">
              <strong>Mood:</strong> {entry.mood}
            </div>
          </div>
        {/if}

        <!-- Gym -->
        {#if entry.gym}
          <div class="activity-item">
            <span class="activity-icon">💪</span>
            <div class="activity-content">
              <strong>Gym:</strong> {entry.gym.didGym ? `Yes, ${entry.gym.duration} min` : "No"}
            </div>
          </div>
        {/if}

        <!-- Yoga -->
        {#if entry.yoga}
          <div class="activity-item">
            <span class="activity-icon">🧘</span>
            <div class="activity-content">
              <strong>Yoga:</strong> {formatDuration(entry.yoga.duration)}
              {#if entry.yoga.exercises && entry.yoga.exercises.length > 0}
                <div class="sub-details">Exercises: {entry.yoga.exercises.join(", ")}</div>
              {/if}
            </div>
          </div>
        {/if}

        <!-- Water -->
        {#if entry.water}
          <div class="activity-item">
            <span class="activity-icon">💧</span>
            <div class="activity-content">
              <strong>Water:</strong> {formatWater(entry.water)}
            </div>
          </div>
        {/if}

        <!-- Energy -->
        {#if entry.energy}
          <div class="activity-item">
            <span class="activity-icon">⚡</span>
            <div class="activity-content">
              <strong>Energy:</strong> {entry.energy.charAt(0).toUpperCase() + entry.energy.slice(1)}
            </div>
          </div>
        {/if}

        <!-- Exercise -->
        {#if entry.exercise}
          <div class="activity-item">
            <span class="activity-icon">🏃</span>
            <div class="activity-content">
              <strong>Exercise:</strong>
              {#if entry.exercise.tookWalk || entry.exercise.wentRunning}
                <div class="sub-details">
                  {#if entry.exercise.tookWalk}• Took a walk{/if}
                  {#if entry.exercise.wentRunning}• Went for a run{/if}
                  {#if entry.exercise.distance > 0}• Distance: {entry.exercise.distance} miles/km{/if}
                </div>
              {:else}
                No exercise recorded
              {/if}
            </div>
          </div>
        {/if}
      </div>

      <!-- Post/Caption Section -->
      {#if entry.image || entry.caption}
        <div class="post-section">
          <div class="post-header">
            <span class="activity-icon">📝</span>
            <strong>Post about your day:</strong>
          </div>
          
          {#if entry.image}
            <img src={entry.image} alt="entry" class="entry-image" />
          {/if}
          
          <div class="caption-section">
            {#if editingId === entry.id}
              <textarea bind:value={editedCaption} placeholder="Write about your day..."></textarea>
              <button class="save-btn" on:click={() => saveEdit(entry.id)}>Save</button>
              <button class="cancel-btn" on:click={() => editingId = null}>Cancel</button>
            {:else}
              <p class="caption-text">{entry.caption || "No caption added"}</p>
              <button class="edit-btn" on:click={() => startEdit(entry)}>Edit</button>
            {/if}
          </div>
        </div>
      {/if}
    </div>
  {/each}
</div>

<style>
  .entries {
    display: flex;
    flex-direction: column;
    gap: 2rem;
    margin-top: 1rem;
    padding: 1rem;
  }

  .entry-card {
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(76, 175, 80, 0.2);
    border-radius: 20px;
    padding: 2rem;
    box-shadow: 0 8px 32px rgba(76, 175, 80, 0.1);
    transition: all 0.3s ease;
  }

  .entry-card:hover {
    box-shadow: 0 12px 40px rgba(76, 175, 80, 0.15);
    border-color: var(--green-primary);
  }

  .entry-header h3 {
    font-size: 1.5rem;
    color: var(--green-primary);
    margin-bottom: 1.5rem;
    text-align: center;
  }

  .activities-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1rem;
    margin-bottom: 1.5rem;
  }

  .activity-item {
    display: flex;
    align-items: flex-start;
    gap: 0.75rem;
    padding: 1rem;
    background: rgba(76, 175, 80, 0.05);
    border-radius: 12px;
    border: 1px solid rgba(76, 175, 80, 0.1);
  }

  .activity-icon {
    font-size: 1.5rem;
    min-width: 1.5rem;
  }

  .activity-content {
    flex: 1;
  }

  .activity-content strong {
    color: var(--green-dark);
  }

  .sub-details {
    font-size: 0.9rem;
    color: #666;
    margin-top: 0.25rem;
    font-style: italic;
  }

  .post-section {
    border-top: 2px solid rgba(76, 175, 80, 0.2);
    padding-top: 1.5rem;
    margin-top: 1.5rem;
  }

  .post-header {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    margin-bottom: 1rem;
    font-size: 1.1rem;
  }

  .post-header strong {
    color: var(--green-primary);
  }

  .entry-image {
    width: 100%;
    max-width: 300px;
    height: auto;
    border-radius: 12px;
    margin-bottom: 1rem;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  }

  .caption-section {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }

  .caption-text {
    background: rgba(76, 175, 80, 0.05);
    padding: 1rem;
    border-radius: 8px;
    border-left: 4px solid var(--green-primary);
    margin: 0;
    line-height: 1.6;
  }

  textarea {
    width: 100%;
    min-height: 80px;
    padding: 1rem;
    border: 2px solid rgba(76, 175, 80, 0.2);
    border-radius: 8px;
    font-family: inherit;
    font-size: 1rem;
    resize: vertical;
    background: rgba(255, 255, 255, 0.9);
  }

  textarea:focus {
    outline: none;
    border-color: var(--green-primary);
    box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.1);
  }

  button {
    padding: 0.5rem 1rem;
    border-radius: 8px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s ease;
    border: none;
    font-size: 0.9rem;
  }

  .edit-btn {
    background: var(--primary-blue);
    color: white;
    align-self: flex-start;
  }

  .edit-btn:hover {
    background: var(--dark-blue);
    transform: translateY(-1px);
  }

  .save-btn {
    background: #4CAF50;
    color: white;
  }

  .save-btn:hover {
    background: #45a049;
  }

  .cancel-btn {
    background: #f44336;
    color: white;
  }

  .cancel-btn:hover {
    background: #d32f2f;
  }

  @media (max-width: 768px) {
    .entries {
      padding: 0.5rem;
    }

    .entry-card {
      padding: 1.5rem;
    }

    .activities-grid {
      grid-template-columns: 1fr;
    }

    .entry-image {
      max-width: 100%;
    }
  }
</style>
