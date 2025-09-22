<script>
  export let energyLevel = "";
</script>

<label><input type="radio" bind:group={energyLevel} value="low" /> Low</label>
<label><input type="radio" bind:group={energyLevel} value="medium" /> Medium</label>
<label><input type="radio" bind:group={energyLevel} value="high" /> High</label>
