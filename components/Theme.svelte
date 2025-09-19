<script>
  export let theme = 'light';
  export let lastTheme = null;

  function setTheme(newTheme) {
    lastTheme = theme;
    theme = newTheme;
    document.body.className = newTheme;
  }

  function undoTheme() {
    if (lastTheme) {
      theme = lastTheme;
      document.body.className = lastTheme;
      lastTheme = null;
    }
  }
</script>

<div>
  <label>Theme:
    <select bind:value={theme} on:change={() => setTheme(theme)}>
      <option value="light">Light</option>
      <option value="dark">Dark</option>
      <option value="blue">Blue</option>
    </select>
  </label>
  <button on:click={undoTheme} disabled={!lastTheme}>Undo Theme Change</button>
</div>