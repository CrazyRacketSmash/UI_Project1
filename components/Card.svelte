<script>
  export let title;
</script>

<section class="card">
  <h2>{title}</h2>
  <slot />
</section>

<style>
  .card {
    background: white;
    border-radius: 14px;
    padding: 1.5rem;
    margin: 1rem 0;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  }
  h2 {
    margin-bottom: 1rem;
  }
</style>
