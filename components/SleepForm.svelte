<script>
  export let hoursSlept;
</script>

<label>
  Hours Slept:
  <input type="number" bind:value={hoursSlept} min="0" max="12" />
</label>
