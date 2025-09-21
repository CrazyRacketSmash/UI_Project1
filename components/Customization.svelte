<script>
  export let showCustomize = false;
  export let allActivities = [
    { key: 'sleep', label: 'Sleep' },
    { key: 'gym', label: 'Gym' },
    { key: 'mood', label: 'Mood' },
    { key: 'yoga', label: 'Yoga' },
    { key: 'water', label: 'Water Intake' }
  ];
  export let activeActivities = ['sleep', 'gym', 'mood'];
  export let lastCustomization = null;

  function toggleActivity(key) {
    if (activeActivities.includes(key)) {
      activeActivities = activeActivities.filter(a => a !== key);
      lastCustomization = { type: 'remove', key };
    } else {
      activeActivities = [...activeActivities, key];
      lastCustomization = { type: 'add', key };
    }
  }

  function undoCustomization() {
    if (!lastCustomization) return;
    if (lastCustomization.type === 'add') {
      activeActivities = activeActivities.filter(a => a !== lastCustomization.key);
    } else {
      activeActivities = [...activeActivities, lastCustomization.key];
    }
    lastCustomization = null;
  }
</script>

<button on:click={() => showCustomize = true}>Customize Activities</button>

{#if showCustomize}
  <div class="overlay">
    <div class="modal">
      <h2>Choose Activities</h2>
      {#each allActivities as activity}
        <label>
          <input
            type="checkbox"
            checked={activeActivities.includes(activity.key)}
            on:change={() => toggleActivity(activity.key)}
          />
          {activity.label}
        </label>
      {/each}
      <button on:click={() => showCustomize = false}>OK</button>
      <button on:click={undoCustomization} disabled={!lastCustomization}>Undo</button>
      {#if lastCustomization}
        <p>
          {lastCustomization.type === 'add'
            ? `Added ${lastCustomization.key}!`
            : `Removed ${lastCustomization.key}!`}
        </p>
      {/if}
    </div>
  </div>
{/if}

