<script>
  export let tookWalk = false;
  export let wentRunning = false;
  export let distance = 0;
</script>

<label><input type="checkbox" bind:checked={tookWalk} /> Took a walk today</label>
<label><input type="checkbox" bind:checked={wentRunning} /> Went for a run</label>
<label>
  Distance (miles):
  <input type="number" min="0" step="0.1" bind:value={distance} />
</label>
