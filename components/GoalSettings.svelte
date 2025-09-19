<script>
  let showGoalSettings = false;
  let goals = {
    sleep: 7,
    gym: 30
  };

  function saveGoals() {
    showGoalSettings = false;
    alert('Goals updated!');
  }
</script>

<button on:click={() => showGoalSettings = !showGoalSettings}>
  {showGoalSettings ? "Close Goal Settings" : "Set Goals"}
</button>

{#if showGoalSettings}
  <div class="goal-settings">
    <label>
      Sleep Goal (hours):
      <input type="number" bind:value={goals.sleep} min="0" max="12" />
    </label>
    <label>
      Gym Goal (minutes):
      <input type="number" bind:value={goals.gym} min="0" max="180" />
    </label>
    <button on:click={saveGoals}>Save Goals</button>
  </div>
{/if}