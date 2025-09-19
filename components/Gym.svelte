<script>
  export let wentToGym = false;
  export let gymDuration = 0;
</script>

<label>
  <input type="checkbox" bind:checked={wentToGym} />
  Went to the gym today
</label>

{#if wentToGym}
  <label>
    Duration (minutes):
    <input type="number" min="0" bind:value={gymDuration} />
  </label>
{/if}

<style>
  label {
    display: block;
    margin: 0.5rem 0;
  }
  input[type="number"] {
    width: 80px;
    margin-left: 0.5rem;
  }
</style>
