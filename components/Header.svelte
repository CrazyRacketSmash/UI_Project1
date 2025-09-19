<script>
  export let name;
</script>

<div class="header">
  <h1>Activity Tracker</h1>
  <p>Welcome back, {name}</p>
  <p>{new Date().toLocaleString()}</p>
</div>

<style>
  .header {
    text-align: center;
    margin-bottom: 1rem;
  }
  h1 {
    margin: 0;
    font-size: 2.2rem;
  }
  p {
    color: #555;
    margin: 0.3rem 0;
  }
</style>
