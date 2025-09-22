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

{#if showCustomize}
  <div class="overlay">
    <div class="modal">
      <div class="activities-list">
        {#each allActivities as activity}
          <div class="activity-item">
            <span class="activity-label">{activity.label}</span>
            <label class="slider-container">
              <input
                type="checkbox"
                checked={activeActivities.includes(activity.key)}
                on:change={() => toggleActivity(activity.key)}
              />
              <span class="slider"></span>
            </label>
          </div>
        {/each}
      </div>
      <div class="button-group">
        <button on:click={() => showCustomize = false}>OK</button>
        <button on:click={undoCustomization} disabled={!lastCustomization}>Undo</button>
      </div>
      {#if lastCustomization}
        <p class="feedback-message">
          {lastCustomization.type === 'add'
            ? `Added ${lastCustomization.key}!`
            : `Removed ${lastCustomization.key}!`}
        </p>
      {/if}
    </div>
  </div>
{/if}

<style>
  .activities-list {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    margin: 1.5rem 0;
  }

  .activity-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.5rem;
    background: rgba(255, 255, 255, 0.5);
    border-radius: 12px;
    border: 1px solid rgba(38, 166, 154, 0.2);
    transition: all 0.3s ease;
  }

  .activity-item:hover {
    background: rgba(230, 255, 230, 0.7);
    border-color: rgba(38, 166, 154, 0.4);
  }

  .activity-label {
    font-weight: 500;
    color: #00695c;
    font-size: 1rem;
  }

  .slider-container {
    position: relative;
    display: inline-block;
    width: 40px;
    height: 20px;
    cursor: pointer;
  }

  .slider-container input {
    opacity: 0;
    width: 0;
    height: 0;
  }

  .slider {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: #ccc;
    border-radius: 20px;
    transition: all 0.3s ease;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
  }

  .slider:before {
    position: absolute;
    content: "";
    height: 16px;
    width: 16px;
    left: 2px;
    bottom: 2px;
    background: white;
    border-radius: 50%;
    transition: all 0.3s ease;
    box-shadow: 0 2px 4px rgba(0,0,0,0.2);
  }

  .slider-container input:checked + .slider {
    background: linear-gradient(45deg, #00c851, #26a69a);
  }

  .slider-container input:checked + .slider:before {
    transform: translateX(20px);
  }

  .slider:hover {
    box-shadow: 0 0 8px rgba(0, 200, 81, 0.3);
  }

  .button-group {
    display: flex;
    gap: 1rem;
    margin-top: 1.5rem;
  }

  .button-group button {
    flex: 1;
    padding: 0.75rem;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.3s ease;
  }

  .button-group button:first-child {
    background: linear-gradient(45deg, #00c851, #26a69a);
    color: white;
  }

  .button-group button:first-child:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 200, 81, 0.3);
  }

  .button-group button:last-child {
    background: #f5f5f5;
    color: #666;
  }

  .button-group button:last-child:hover:not(:disabled) {
    background: #e0e0e0;
  }

  .button-group button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  .feedback-message {
    position: fixed;
    top: 20px;
    right: 20px;
    padding: 1rem 1.5rem;
    background: rgba(0, 200, 81, 0.95);
    color: white;
    border-radius: 12px;
    box-shadow: 0 6px 20px rgba(0, 200, 81, 0.4);
    font-weight: 600;
    font-size: 0.95rem;
    z-index: 2000;
    animation: slideInAlert 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
  }

  @keyframes slideInAlert {
    from {
      transform: translateX(100%) scale(0.8);
      opacity: 0;
    }
    to {
      transform: translateX(0) scale(1);
      opacity: 1;
    }
  }
</style>

