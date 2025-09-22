<script>
// @ts-nocheck

  export let selectedMoods = [];
  const moods = [
    "Happy", "Excited", "Relaxed", "Grateful", "Focused", "Energetic",
    "Stressed", "Anxious", "Tired", "Sad", "Lonely", "Angry"
  ];

  function toggleMood(mood, checked) {
    if (checked) {
      selectedMoods = [...selectedMoods, mood];
    } else {
      selectedMoods = selectedMoods.filter(m => m !== mood);
    }
  }
</script>

<div class="mood-grid">
  {#each moods as mood}
    <label>
      <input
        type="checkbox"
        checked={selectedMoods.includes(mood)}
        on:change={(e) => toggleMood(mood, e.target.checked)}
      />
      {mood}
    </label>
  {/each}
</div>

<style>
  .mood-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 0.5rem;
  }
  label {
    display: flex;
    align-items: center;
    gap: 0.4rem;
  }
</style>
