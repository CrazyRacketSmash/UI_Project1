<script>
  export let entries = [];
  
  let editingId = null;
  let editingField = null;
  
  // Edit state for all activity types
  let editData = {
    sleep: { hours: 0, quality: false },
    mood: "",
    gym: { didGym: false, duration: 0 },
    yoga: { duration: 0, exercises: "" },
    water: { cups: 0, metGoal: false },
    energy: "",
    exercise: { tookWalk: false, wentRunning: false, distance: 0 },
    caption: ""
  };

  function startEdit(entry, field) {
    editingId = entry.id;
    editingField = field;
    
    // Init edit data based on field
    switch(field) {
      case 'sleep':
        editData.sleep = { ...entry.sleep };
        break;
      case 'mood':
        editData.mood = entry.mood || "";
        break;
      case 'gym':
        editData.gym = { ...entry.gym };
        break;
      case 'yoga':
        editData.yoga = { 
          duration: entry.yoga?.duration || 0,
          exercises: (entry.yoga?.exercises || []).join(", ")
        };
        break;
      case 'water':
        editData.water = { ...entry.water };
        break;
      case 'energy':
        editData.energy = entry.energy || "";
        break;
      case 'exercise':
        editData.exercise = { ...entry.exercise };
        break;
      case 'caption':
        editData.caption = entry.caption || "";
        break;
    }
  }

  function saveEdit(id) {
    const entry = entries.find(e => e.id === id);
    if (entry && editingField) {
      // Update the specific field
      switch(editingField) {
        case 'sleep':
          entry.sleep = { ...editData.sleep };
          break;
        case 'mood':
          entry.mood = editData.mood;
          break;
        case 'gym':
          entry.gym = { ...editData.gym };
          break;
        case 'yoga':
          entry.yoga = { 
            duration: editData.yoga.duration,
            exercises: editData.yoga.exercises.split(",").map(e => e.trim()).filter(e => e.length > 0)
          };
          break;
        case 'water':
          entry.water = { ...editData.water };
          break;
        case 'energy':
          entry.energy = editData.energy;
          break;
        case 'exercise':
          entry.exercise = { ...editData.exercise };
          break;
        case 'caption':
          entry.caption = editData.caption;
          break;
      }
      
      cancelEdit();
      alert(`${editingField.charAt(0).toUpperCase() + editingField.slice(1)} updated!`);
    }
  }

  function cancelEdit() {
    editingId = null;
    editingField = null;
  }

  function formatDuration(duration) {
    return duration && duration > 0 ? `${duration} min` : "Not recorded";
  }

  function formatSleep(sleep) {
    if (!sleep) return "Not recorded";
    return `${sleep.hours || 0}h`;
  }

  function formatWater(water) {
    if (!water) return "Not recorded";
    return `${water.cups || 0} cups`;
  }
</script>

<div class="entries">
  {#each entries as entry}
    <div class="entry-card">
      <div class="entry-header">
        <h3>{entry.date}</h3>
      </div>
      
      <div class="activities-grid">
        <!-- Sleep -->
        {#if entry.sleep}
          <div class="activity-item">
            <div class="activity-content">
              <strong>Sleep:</strong>
              {#if editingId === entry.id && editingField === 'sleep'}
                <div class="edit-form">
                  <label>Hours: <input type="number" bind:value={editData.sleep.hours} min="0" max="24" /></label>
                  <label><input type="checkbox" bind:checked={editData.sleep.quality} /> Slept well</label>
                  <div class="edit-buttons">
                    <button class="save-btn" on:click={() => saveEdit(entry.id)}>Save</button>
                    <button class="cancel-btn" on:click={cancelEdit}>Cancel</button>
                  </div>
                </div>
              {:else}
                {formatSleep(entry.sleep)}
                <button class="edit-btn-small" on:click={() => startEdit(entry, 'sleep')}>Edit</button>
              {/if}
            </div>
          </div>
        {/if}

        <!-- Mood -->
        {#if entry.mood}
          <div class="activity-item">
            <div class="activity-content">
              <strong>Mood:</strong>
              {#if editingId === entry.id && editingField === 'mood'}
                <div class="edit-form">
                  <input type="text" bind:value={editData.mood} placeholder="Enter mood..." />
                  <div class="edit-buttons">
                    <button class="save-btn" on:click={() => saveEdit(entry.id)}>Save</button>
                    <button class="cancel-btn" on:click={cancelEdit}>Cancel</button>
                  </div>
                </div>
              {:else}
                {entry.mood}
                <button class="edit-btn-small" on:click={() => startEdit(entry, 'mood')}>Edit</button>
              {/if}
            </div>
          </div>
        {/if}

        <!-- Gym -->
        {#if entry.gym}
          <div class="activity-item">
            <div class="activity-content">
              <strong>Gym:</strong>
              {#if editingId === entry.id && editingField === 'gym'}
                <div class="edit-form">
                  <label><input type="checkbox" bind:checked={editData.gym.didGym} /> Went to gym</label>
                  {#if editData.gym.didGym}
                    <label>Duration (min): <input type="number" bind:value={editData.gym.duration} min="0" /></label>
                  {/if}
                  <div class="edit-buttons">
                    <button class="save-btn" on:click={() => saveEdit(entry.id)}>Save</button>
                    <button class="cancel-btn" on:click={cancelEdit}>Cancel</button>
                  </div>
                </div>
              {:else}
                {entry.gym.didGym ? `Yes, ${entry.gym.duration} min` : "No"}
                <button class="edit-btn-small" on:click={() => startEdit(entry, 'gym')}>Edit</button>
              {/if}
            </div>
          </div>
        {/if}

        <!-- Yoga -->
        {#if entry.yoga}
          <div class="activity-item">
            <div class="activity-content">
              <strong>Yoga:</strong>
              {#if editingId === entry.id && editingField === 'yoga'}
                <div class="edit-form">
                  <label>Duration (min): <input type="number" bind:value={editData.yoga.duration} min="0" /></label>
                  <label>Exercises: <input type="text" bind:value={editData.yoga.exercises} placeholder="Enter exercises (comma separated)" /></label>
                  <div class="edit-buttons">
                    <button class="save-btn" on:click={() => saveEdit(entry.id)}>Save</button>
                    <button class="cancel-btn" on:click={cancelEdit}>Cancel</button>
                  </div>
                </div>
              {:else}
                {formatDuration(entry.yoga.duration)}
                {#if entry.yoga.exercises && entry.yoga.exercises.length > 0}
                  <div class="sub-details">Exercises: {entry.yoga.exercises.join(", ")}</div>
                {/if}
                <button class="edit-btn-small" on:click={() => startEdit(entry, 'yoga')}>Edit</button>
              {/if}
            </div>
          </div>
        {/if}

        <!-- Water -->
        {#if entry.water}
          <div class="activity-item">
            <div class="activity-content">
              <strong>Water:</strong>
              {#if editingId === entry.id && editingField === 'water'}
                <div class="edit-form">
                  <label>Cups: <input type="number" bind:value={editData.water.cups} min="0" /></label>
                  <label><input type="checkbox" bind:checked={editData.water.metGoal} /> Met water goal</label>
                  <div class="edit-buttons">
                    <button class="save-btn" on:click={() => saveEdit(entry.id)}>Save</button>
                    <button class="cancel-btn" on:click={cancelEdit}>Cancel</button>
                  </div>
                </div>
              {:else}
                {formatWater(entry.water)}
                <button class="edit-btn-small" on:click={() => startEdit(entry, 'water')}>Edit</button>
              {/if}
            </div>
          </div>
        {/if}

        <!-- Energy -->
        {#if entry.energy}
          <div class="activity-item">
            <div class="activity-content">
              <strong>Energy:</strong>
              {#if editingId === entry.id && editingField === 'energy'}
                <div class="edit-form">
                  <select bind:value={editData.energy}>
                    <option value="">Select energy level</option>
                    <option value="low">Low</option>
                    <option value="medium">Medium</option>
                    <option value="high">High</option>
                  </select>
                  <div class="edit-buttons">
                    <button class="save-btn" on:click={() => saveEdit(entry.id)}>Save</button>
                    <button class="cancel-btn" on:click={cancelEdit}>Cancel</button>
                  </div>
                </div>
              {:else}
                {entry.energy.charAt(0).toUpperCase() + entry.energy.slice(1)}
                <button class="edit-btn-small" on:click={() => startEdit(entry, 'energy')}>Edit</button>
              {/if}
            </div>
          </div>
        {/if}

        <!-- Exercise -->
        {#if entry.exercise}
          <div class="activity-item">
            <div class="activity-content">
              <strong>Exercise:</strong>
              {#if editingId === entry.id && editingField === 'exercise'}
                <div class="edit-form">
                  <label><input type="checkbox" bind:checked={editData.exercise.tookWalk} /> Took a walk</label>
                  <label><input type="checkbox" bind:checked={editData.exercise.wentRunning} /> Went for a run</label>
                  <label>Distance: <input type="number" bind:value={editData.exercise.distance} min="0" step="0.1" /> miles/km</label>
                  <div class="edit-buttons">
                    <button class="save-btn" on:click={() => saveEdit(entry.id)}>Save</button>
                    <button class="cancel-btn" on:click={cancelEdit}>Cancel</button>
                  </div>
                </div>
              {:else}
                {#if entry.exercise.tookWalk || entry.exercise.wentRunning}
                  <div class="sub-details">
                    {#if entry.exercise.tookWalk}• Took a walk{/if}
                    {#if entry.exercise.wentRunning}• Went for a run{/if}
                    {#if entry.exercise.distance > 0}• Distance: {entry.exercise.distance} miles/km{/if}
                  </div>
                {:else}
                  No exercise recorded
                {/if}
                <button class="edit-btn-small" on:click={() => startEdit(entry, 'exercise')}>Edit</button>
              {/if}
            </div>
          </div>
        {/if}
      </div>

      <!-- Post/Caption Section -->
      {#if entry.image || entry.caption}
        <div class="post-section">
          <div class="post-header">
            <strong>Post about your day:</strong>
          </div>
          
          {#if entry.image}
            <img src={entry.image} alt="entry" class="entry-image" />
          {/if}
          
          <div class="caption-section">
            {#if editingId === entry.id && editingField === 'caption'}
              <textarea bind:value={editData.caption} placeholder="Write about your day..."></textarea>
              <div class="edit-buttons">
                <button class="save-btn" on:click={() => saveEdit(entry.id)}>Save</button>
                <button class="cancel-btn" on:click={cancelEdit}>Cancel</button>
              </div>
            {:else}
              <p class="caption-text">{entry.caption || "No caption added"}</p>
              <button class="edit-btn" on:click={() => startEdit(entry, 'caption')}>Edit</button>
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

  .activity-content {
    flex: 1;
    position: relative;
  }

  .edit-btn-small {
    background: var(--primary-blue);
    color: white;
    border: none;
    border-radius: 50%;
    width: 24px;
    height: 24px;
    font-size: 0.8rem;
    cursor: pointer;
    position: absolute;
    top: 0;
    right: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
  }

  .edit-btn-small:hover {
    background: var(--dark-blue);
    transform: scale(1.1);
  }

  .edit-form {
    background: rgba(255, 255, 255, 0.9);
    padding: 1rem;
    border-radius: 8px;
    border: 2px solid var(--green-primary);
    margin-top: 0.5rem;
  }

  .edit-form label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 500;
  }

  .edit-form input,
  .edit-form select,

  .edit-form input[type="checkbox"] {
    width: auto;
    margin-right: 0.5rem;
  }

  .edit-buttons {
    display: flex;
    gap: 0.5rem;
    margin-top: 1rem;
  }

  .edit-buttons button {
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
