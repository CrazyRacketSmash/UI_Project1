<script>
  export let theme = 'green';
  export let lastTheme = null;

  function toggleTheme() {
    lastTheme = theme;
    theme = theme === 'green' ? 'purple' : 'green';
    applyTheme(theme);
  }

  function applyTheme(newTheme) {
    const root = document.documentElement;
    
    if (newTheme === 'green') {
      // Primary theme colors
      root.style.setProperty('--primary-color', '#4CAF50');
      root.style.setProperty('--secondary-color', '#66BB6A');
      root.style.setProperty('--accent-color', '#81C784');
      root.style.setProperty('--shadow-color', 'rgba(76, 175, 80, 0.2)');
      
      // Background gradients
      root.style.setProperty('--bg-gradient-start', '#E8F5E8');
      root.style.setProperty('--bg-gradient-end', '#F1F8E9');
      
      // Progress bar colors
      root.style.setProperty('--progress-bg', 'linear-gradient(135deg, rgba(76, 175, 80, 0.1), rgba(102, 187, 106, 0.1))');
      root.style.setProperty('--progress-track', 'linear-gradient(90deg, rgba(76, 175, 80, 0.1), rgba(102, 187, 106, 0.1))');
      
      // Legacy green variables for compatibility
      root.style.setProperty('--green-primary', '#4CAF50');
      root.style.setProperty('--green-secondary', '#66BB6A');
      root.style.setProperty('--green-dark', '#388E3C');
    } else if (newTheme === 'purple') {
      // Primary theme colors
      root.style.setProperty('--primary-color', '#9C27B0');
      root.style.setProperty('--secondary-color', '#BA68C8');
      root.style.setProperty('--accent-color', '#CE93D8');
      root.style.setProperty('--shadow-color', 'rgba(156, 39, 176, 0.2)');
      
      // Background gradients
      root.style.setProperty('--bg-gradient-start', '#F3E5F5');
      root.style.setProperty('--bg-gradient-end', '#E1BEE7');
      
      // Progress bar colors
      root.style.setProperty('--progress-bg', 'linear-gradient(135deg, rgba(156, 39, 176, 0.1), rgba(186, 104, 200, 0.1))');
      root.style.setProperty('--progress-track', 'linear-gradient(90deg, rgba(156, 39, 176, 0.1), rgba(186, 104, 200, 0.1))');
      
      // Legacy green variables (override with purple for consistency)
      root.style.setProperty('--green-primary', '#9C27B0');
      root.style.setProperty('--green-secondary', '#BA68C8');
      root.style.setProperty('--green-dark', '#7B1FA2');
    }

    // Apply background to body
    document.body.style.background = `linear-gradient(135deg, var(--bg-gradient-start), var(--bg-gradient-end))`;
    document.body.style.minHeight = '100vh';
  }
  // Apply theme on component mount
  import { onMount } from 'svelte';
  onMount(() => {
    console.log('Initial theme:', theme); // Debug log
    applyTheme(theme);
  });
</script>

<div class="theme-switcher">
  <button class="theme-toggle-btn {theme}" on:click={toggleTheme}>
    <div class="theme-info">
      <span class="theme-label">Theme</span>
      <span class="theme-name">{theme === 'green' ? 'Nature Green' : 'Mystic Purple'}</span>
    </div>
    <div class="switch-indicator">
      <div class="switch-dot {theme}"></div>
    </div>
  </button>
</div>

<style>
  .theme-switcher {
    position: fixed;
    top: 1rem;
    left: 1rem;
    z-index: 1000;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .theme-toggle-btn {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    padding: 0.75rem 1rem;
    border: none;
    border-radius: 16px;
    cursor: pointer;
    transition: all 0.4s ease;
    font-family: inherit;
    min-width: 140px;
    position: relative;
    overflow: hidden;
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
  }

  .theme-toggle-btn.green {
    background: linear-gradient(135deg, #4CAF50, #66BB6A);
    box-shadow: 0 8px 25px rgba(76, 175, 80, 0.3);
    color: white;
  }

  .theme-toggle-btn.purple {
    background: linear-gradient(135deg, #9C27B0, #BA68C8);
    box-shadow: 0 8px 25px rgba(156, 39, 176, 0.3);
    color: white;
  }

  .theme-toggle-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 12px 35px rgba(0, 0, 0, 0.2);
  }

  .theme-toggle-btn::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
    transition: left 0.6s ease;
  }

  .theme-toggle-btn:hover::before {
    left: 100%;
  }

  .theme-info {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    flex: 1;
  }

  .theme-label {
    font-size: 0.8rem;
    opacity: 0.9;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }

  .theme-name {
    font-size: 1.1rem;
    font-weight: 700;
    margin-top: 0.2rem;
  }

  .switch-indicator {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 50px;
    height: 24px;
    background: rgba(255, 255, 255, 0.2);
    border-radius: 12px;
    position: relative;
  }

  .switch-dot {
    width: 18px;
    height: 18px;
    border-radius: 50%;
    background: white;
    transition: transform 0.3s ease;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  }

  .switch-dot.green {
    transform: translateX(-13px);
  }

  .switch-dot.purple {
    transform: translateX(13px);
  }

  .undo-btn {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.75rem 1rem;
    background: rgba(255, 255, 255, 0.9);
    border: 2px solid rgba(0, 0, 0, 0.1);
    border-radius: 12px;
    cursor: pointer;
    transition: all 0.3s ease;
    font-weight: 600;
    color: #666;
    backdrop-filter: blur(10px);
  }

  .undo-btn:hover {
    background: rgba(255, 255, 255, 1);
    border-color: var(--primary-color);
    color: var(--primary-color);
    transform: translateY(-1px);
  }

  .undo-icon {
    font-size: 1.2rem;
    animation: spin 2s linear infinite;
  }

  @keyframes float {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-3px); }
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  @media (max-width: 480px) {
    .theme-switcher {
      flex-direction: column;
      align-items: stretch;
    }

    .theme-toggle-btn {
      min-width: auto;
    }
  }
</style>