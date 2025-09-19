<script>
  export let hoursSlept;
  export let sleptWell;
</script>

<label>
  Hours Slept:
  <input type="number" bind:value={hoursSlept} min="0" max="12" />
</label>
<label>
  <input type="checkbox" bind:checked={sleptWell} />
  Slept well (quality)
</label>
