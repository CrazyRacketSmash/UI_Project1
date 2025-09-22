<script>
  export let showGoalSettings = false;
  export let goals = {
    sleep: 7,
    gym: 30,
    yoga: 20,
    water: 8,
    exercise: 1
  };

  let showSuccessToast = false;
  let isLoading = false;

  async function saveGoals() {
    isLoading = true;
    // Simulate saving delay
    await new Promise(resolve => setTimeout(resolve, 800));
    
    isLoading = false;
    showGoalSettings = false;
    showSuccessToast = true;
    
    // Hide toast after 3 seconds
    setTimeout(() => {
      showSuccessToast = false;
    }, 3000);
  }
</script>


{#if showGoalSettings}
  <div class="overlay">
    <div class="modal">
      <div class="goals-grid">
        <!-- Goals -->
        <div class="goal-category">          
          <div class="goal-item">
            <div class="goal-header">
              <span class="goal-label">Sleep Goal</span>
            </div>
            <div class="goal-input-group">
              <input type="number" bind:value={goals.sleep} min="0" max="12" step="0.5" />
              <span class="goal-unit">hours</span>
            </div>
          </div>

          <div class="goal-item">
            <div class="goal-header">
              <span class="goal-label">Gym Goal</span>
            </div>
            <div class="goal-input-group">
              <input type="number" bind:value={goals.gym} min="0" max="180" step="5" />
              <span class="goal-unit">minutes</span>
            </div>
          </div>

          <div class="goal-item">
            <div class="goal-header">
              <span class="goal-label">Yoga Goal</span>
            </div>
            <div class="goal-input-group">
              <input type="number" bind:value={goals.yoga} min="0" max="120" step="5" />
              <span class="goal-unit">minutes</span>
            </div>
          </div>

          <div class="goal-item">
            <div class="goal-header">
              <span class="goal-label">Water Goal</span>
            </div>
            <div class="goal-input-group">
              <input type="number" bind:value={goals.water} min="0" max="20" />
              <span class="goal-unit">cups</span>
            </div>
          </div>
        </div>
      </div>
      
      <div class="button-group">
        <button class="save-btn" on:click={saveGoals} disabled={isLoading}>
          {#if isLoading}
            <span class="loading-spinner"></span>
            Saving Goals...
          {:else}
            Save Goals
          {/if}
        </button>
        <button class="cancel-btn" on:click={() => showGoalSettings = false} disabled={isLoading}>
          Cancel
        </button>
      </div>
    </div>
  </div>
{/if}

<!-- Notification for success -->
{#if showSuccessToast}
  <div class="success-toast">
    <div class="toast-content">
      <span class="toast-message">Goals updated successfully!</span>
      <button class="toast-close" on:click={() => showSuccessToast = false}>×</button>
    </div>
  </div>
{/if}

<style>
  .goals-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 2rem;
    margin-bottom: 2rem;
  }

  .goal-category {
    padding: 1.5rem;
  }

  .goal-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    margin-bottom: 1rem;
    background: rgba(255, 255, 255, 0.9);
    border-radius: 12px;
    border: 1px solid rgba(76, 175, 80, 0.1);
    transition: all 0.3s ease;
  }

  .goal-item:hover {
    border-color: var(--green-primary);
    box-shadow: 0 4px 12px rgba(76, 175, 80, 0.1);
  }

  .goal-item:last-child {
    margin-bottom: 0;
  }

  .goal-header {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    flex: 1;
  }

  .goal-label {
    font-weight: 600;
    color: var(--green-dark);
    font-size: 1rem;
  }

  .goal-input-group {
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .goal-input-group input {
    width: 70px;
    padding: 0.5rem;
    border: 2px solid rgba(76, 175, 80, 0.2);
    border-radius: 8px;
    text-align: center;
    font-size: 1rem;
    font-weight: 600;
    color: var(--green-dark);
    background: white;
  }

  .goal-input-group input:focus {
    outline: none;
    border-color: var(--green-primary);
    box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.1);
  }

  .goal-unit {
    font-size: 0.9rem;
    color: #666;
    font-weight: 500;
    min-width: 60px;
  }

  .button-group {
    display: flex;
    gap: 1rem;
    margin-top: 2rem;
  }

  .button-group button {
    flex: 1;
    padding: 1rem 1.5rem;
    border: none;
    border-radius: 12px;
    cursor: pointer;
    font-weight: 600;
    font-size: 1rem;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
  }

  .save-btn {
    background: linear-gradient(135deg, #b3e0ff, #6eafcd);
    color: black;
  }

  .save-btn:hover {
    background: linear-gradient(135deg, var(--green-dark), var(--green-primary));
    transform: translateY(-2px);
    box-shadow: 0 8px 25px rgba(76, 175, 80, 0.3);
  }

  .save-btn:disabled {
    opacity: 0.7;
    cursor: not-allowed;
    transform: none;
  }

  .loading-spinner {
    display: inline-block;
    width: 16px;
    height: 16px;
    border: 2px solid rgba(255, 255, 255, 0.3);
    border-top: 2px solid white;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin-right: 0.5rem;
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  .cancel-btn {
    background: grey;
    color: #666;
    border: 2px solid rgba(76, 175, 80, 0.2);
  }

  .cancel-btn:hover {
    background: #e0e0e0;
    border-color: var(--green-primary);
    color: var(--green-dark);
  }

  .cancel-btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  .success-toast {
    position: fixed;
    top: 20px;
    right: 20px;
    z-index: 10000;
    animation: slideInFromRight 0.5s ease-out, fadeOut 0.5s ease-in 2.5s forwards;
  }

  .toast-content {
    background: linear-gradient(135deg, #4c88e3, #a2b4e7);
    color: black;
    padding: 1rem 1.5rem;
    border-radius: 12px;
    box-shadow: 0 8px 32px rgba(76, 175, 80, 0.4);
    display: flex;
    align-items: center;
    gap: 0.75rem;
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
  }

  .toast-message {
    font-weight: 600;
    font-size: 1rem;
  }

  .toast-close {
    background: none;
    border: none;
    color: white;
    font-size: 1.2rem;
    font-weight: bold;
    cursor: pointer;
    padding: 0;
    margin-left: 0.5rem;
    opacity: 0.8;
    transition: opacity 0.3s ease;
  }

  .toast-close:hover {
    opacity: 1;
  }

  @keyframes slideInFromRight {
    0% {
      transform: translateX(100%);
      opacity: 0;
    }
    100% {
      transform: translateX(0);
      opacity: 1;
    }
  }

  @keyframes fadeOut {
    0% {
      opacity: 1;
    }
    100% {
      opacity: 0;
      transform: translateX(100%);
    }
  }

  @keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
      transform: translateY(0);
    }
    40% {
      transform: translateY(-8px);
    }
    60% {
      transform: translateY(-4px);
    }
  }

  @media (max-width: 768px) {
    .goals-grid {
      grid-template-columns: 1fr;
      gap: 1.5rem;
    }

    .goal-category {
      padding: 1rem;
    }

    .goal-item {
      flex-direction: column;
      gap: 0.75rem;
      align-items: stretch;
    }

    .goal-input-group {
      justify-content: center;
    }

    .button-group {
      flex-direction: column;
    }
  }
</style>