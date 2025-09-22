<script>
  export let totalDays;
  export let activeDays;
  $: percent = Math.min((activeDays / totalDays) * 100, 100);
  $: streakLevel = activeDays >= 7 ? 'fire' : activeDays >= 3 ? 'good' : 'start';
</script>

<div class="progress-wrapper">
  <div class="stats-container">
    <div class="stat-item">
      <span class="stat-number">{activeDays}</span>
      <span class="stat-label">Active Days</span>
    </div>
    <div class="streak-indicator">
      {#if streakLevel === 'fire'}
        🔥
      {:else if streakLevel === 'good'}
        ⭐
      {:else}
        🌱
      {/if}
    </div>
    <div class="stat-item">
      <span class="stat-number">{totalDays}</span>
      <span class="stat-label">Total Days</span>
    </div>
  </div>
  
  <div class="progress-container">
    <div class="progress-bar">
      <div class="progress-fill {streakLevel}" style="width: {percent}%;"></div>
      <div class="progress-sparkle"></div>
    </div>
    <div class="progress-text">{percent.toFixed(0)}%</div>
  </div>
</div>

<style>
  .progress-wrapper {
    background: linear-gradient(135deg, rgba(255,255,255,0.9), rgba(230,255,230,0.8));
    padding: 1.5rem;
    border-radius: 16px;
    margin: 1rem 0;
    border: 1px solid rgba(38, 166, 154, 0.2);
    box-shadow: 0 4px 16px rgba(0, 105, 92, 0.1);
  }

  .stats-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
  }

  .stat-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.25rem;
  }

  .stat-number {
    font-size: 1.8rem;
    font-weight: 700;
    color: #00695c;
    text-shadow: 0 1px 2px rgba(0, 105, 92, 0.2);
  }

  .stat-label {
    font-size: 0.85rem;
    color: #26a69a;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }

  .streak-indicator {
    font-size: 2rem;
    animation: pulse 2s infinite;
  }

  .progress-container {
    position: relative;
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  .progress-bar {
    flex: 1;
    height: 16px;
    background: linear-gradient(90deg, #e0e0e0, #f0f0f0);
    border-radius: 12px;
    overflow: hidden;
    position: relative;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
  }

  .progress-fill {
    height: 100%;
    border-radius: 12px;
    transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
    position: relative;
    overflow: hidden;
  }

  .progress-fill.start {
    background: linear-gradient(90deg, #81c784, #a5d6a7);
  }

  .progress-fill.good {
    background: linear-gradient(90deg, #4ecdc4, #26a69a);
  }

  .progress-fill.fire {
    background: linear-gradient(90deg, #00c851, #00e676, #00c851);
    animation: shimmer 2s infinite;
  }

  .progress-fill::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
    animation: slide 2s infinite;
  }

  .progress-text {
    font-weight: 700;
    color: #00695c;
    font-size: 1.1rem;
    min-width: 40px;
  }

  @keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.1); }
  }

  @keyframes shimmer {
    0%, 100% { background-position: -100% 0; }
    50% { background-position: 100% 0; }
  }

  @keyframes slide {
    0% { transform: translateX(-100%); }
    100% { transform: translateX(100%); }
  }
</style>