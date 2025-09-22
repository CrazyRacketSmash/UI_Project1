<script>
  export let title;
  export let goal = null;
  export let goalUnit = "";
</script>

<section class="card">
  <div class="card-header">
    <h2>{title}</h2>
    {#if goal !== null}
      <div class="goal-badge">
        🎯 Goal: {goal} {goalUnit}
      </div>
    {/if}
  </div>
  <slot />
</section>

<style>
  .card {
    background: white;
    border-radius: 14px;
    padding: 1.5rem;
    margin: 1rem 0;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    border: 1px solid rgba(76, 175, 80, 0.1);
    transition: all 0.3s ease;
  }

  .card:hover {
    border-color: var(--green-primary);
    box-shadow: 0 6px 20px rgba(76, 175, 80, 0.15);
  }

  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  h2 {
    margin: 0;
    color: var(--green-dark);
    font-size: 1.25rem;
  }

  .goal-badge {
    background: linear-gradient(135deg, var(--green-primary), var(--green-secondary));
    color: black;
    padding: 0.4rem 0.8rem;
    border-radius: 20px;
    font-size: 0.8rem;
    font-weight: 600;
    white-space: nowrap;
    box-shadow: 0 2px 8px rgba(76, 175, 80, 0.3);
  }

  @media (max-width: 480px) {
    .card-header {
      flex-direction: column;
      align-items: flex-start;
    }

    .goal-badge {
      align-self: stretch;
      text-align: center;
    }
  }
</style>
