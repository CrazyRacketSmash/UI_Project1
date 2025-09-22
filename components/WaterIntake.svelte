<script>
  export let waterCups = 0;
</script>

<div class="water-log">
  <label>
    Number of Cups:
    <input type="number" min="0" bind:value={waterCups} />
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