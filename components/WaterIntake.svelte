<script>
  export let waterCups = 0;
  export let metWaterGoal = false;
</script>

<div class="water-log">
  <label>
    Water Intake (cups):
    <input type="number" min="0" bind:value={waterCups} />
  </label>
  <label>
    <input type="checkbox" bind:checked={metWaterGoal} />
    Met daily water goal
  </label>
</div>

<style>
.water-log {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
label {
  display: block;
  margin-bottom: 0.5rem;
}
</style>