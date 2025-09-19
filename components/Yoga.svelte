<script>
  export let yogaDuration = 0;
  export let selectedExercises = [];

  const exercises = [
    "Downward Dog",
    "Child's Pose",
    "Warrior II",
    "Tree Pose",
    "Cobra",
    "Bridge",
    "Seated Twist"
  ];

  function toggleExercise(ex) {
    if (selectedExercises.includes(ex)) {
      selectedExercises = selectedExercises.filter(e => e !== ex);
    } else {
      selectedExercises = [...selectedExercises, ex];
    }
  }
</script>

<div class="yoga-log">
  <label>
    Yoga Duration (minutes):
    <input type="number" min="0" bind:value={yogaDuration} />
  </label>

  <fieldset>
    <legend>Yoga Exercises</legend>
    {#each exercises as ex}
      <label>
        <input
          type="checkbox"
          checked={selectedExercises.includes(ex)}
          on:change={() => toggleExercise(ex)}
        />
        {ex}
      </label>
    {/each}
  </fieldset>
</div>

<style>
.yoga-log {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
fieldset {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 0.5rem 1rem;
}
label {
  display: block;
  margin-bottom: 0.5rem;
}
</style>