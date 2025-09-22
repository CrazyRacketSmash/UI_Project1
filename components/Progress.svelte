<script>
  export let totalDays;
  export let activeDays;
  export let onIncreaseTotalDays = null; // increase total days
  export let onDecreaseTotalDays = null; // decrease total days
  $: percent = Math.min((activeDays / totalDays) * 100, 100);
  $: streakLevel = activeDays >= 0.7*totalDays ? 'fire' : activeDays >= 0.3*totalDays ? 'good' : 'start';
</script>

<div class="progress-wrapper">
  <div class="stats-container">
    <div class="stat-item">
      <span class="stat-number">{activeDays}</span>
      <span class="stat-label">Active Days</span>
    </div>
    <div class="streak-indicator">
      {#if streakLevel === 'fire'}
        <img src="/image/fire.png" alt="Fire" />
      {:else if streakLevel === 'good'}
        <img src="/image/star.png" alt="Star" />
      {:else}
        <img src="/image/leaf.png" alt="Leaf" />
      {/if}
    </div>
    <div class="stat-item">
      <div class="total-days-container">
        {#if onDecreaseTotalDays}
          <button class="minus-btn" on:click={onDecreaseTotalDays} title="Remove days">
            -
          </button>
        {/if}
        <span class="stat-number">{totalDays}</span>
        {#if onIncreaseTotalDays}
          <button class="plus-btn" on:click={onIncreaseTotalDays} title="Add more days">
            +
          </button>
        {/if}
      </div>
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
    background: var(--progress-bg, linear-gradient(135deg, rgba(255,255,255,0.9), rgba(230,255,230,0.8)));
    padding: 1.5rem;
    border-radius: 16px;
    margin: 1rem 0;
    border: 1px solid var(--primary-color, #26a69a);
    box-shadow: 0 4px 16px var(--shadow-color, rgba(0, 105, 92, 0.1));
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

  .total-days-container {
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .plus-btn {
    background: var(--primary-color, #00695c);
    color: white;
    border: none;
    border-radius: 50%;
    width: 24px;
    height: 24px;
    font-size: 1rem;
    font-weight: bold;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
    box-shadow: 0 2px 4px var(--shadow-color, rgba(0, 105, 92, 0.2));
  }

  .plus-btn:hover {
    background: var(--secondary-color, #26a69a);
    transform: scale(1.1);
    box-shadow: 0 4px 8px var(--shadow-color, rgba(0, 105, 92, 0.3));
  }

  .plus-btn:active {
    transform: scale(0.95);
  }

  .minus-btn {
    background: var(--accent-color, #4ecdc4);
    color: white;
    border: none;
    border-radius: 50%;
    width: 24px;
    height: 24px;
    font-size: 1.2rem;
    font-weight: bold;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
    box-shadow: 0 2px 4px var(--shadow-color, rgba(0, 105, 92, 0.2));
  }

  .minus-btn:hover {
    background: var(--secondary-color, #26a69a);
    transform: scale(1.1);
    box-shadow: 0 4px 8px var(--shadow-color, rgba(0, 105, 92, 0.3));
  }

  .minus-btn:active {
    transform: scale(0.95);
  }

  .stat-number {
    font-size: 1.8rem;
    font-weight: 700;
    color: var(--primary-color, #00695c);
    text-shadow: 0 1px 2px var(--shadow-color, rgba(0, 105, 92, 0.2));
  }

  .stat-label {
    font-size: 0.85rem;
    color: var(--secondary-color, #26a69a);
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
    background: var(--progress-track, linear-gradient(90deg, #e0e0e0, #f0f0f0));
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
    color: var(--primary-color, #00695c);
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