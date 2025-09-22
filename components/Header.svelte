<script>
  export let name;
  
  // Get current time and update it every second
  let currentTime = new Date();
  setInterval(() => {
    currentTime = new Date();
  }, 1000);

  // Format time with animation
  $: formattedTime = currentTime.toLocaleString('en-US', {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit'
  });
</script>

<div class="header">
  <div class="header-content">
    <h1 class="app-title">
      Daily Routine Tracker
    </h1>
    <div class="welcome-section">
      <p class="welcome-text">
        <span class="greeting">Welcome back,</span>
        <span class="user-name">{name}!</span>
      </p>
      <div class="time-display">
        <span class="current-time">{formattedTime}</span>
      </div>
    </div>
  </div>
</div>

<style>
  .header {
    background: linear-gradient(135deg, var(--green-primary), var(--green-secondary));
    padding: 2rem 1rem;
    text-align: center;
    margin-bottom: 2rem;
    border-radius: 0 0 30px 30px;
    box-shadow: 0 8px 32px rgba(76, 175, 80, 0.3);
    position: relative;
    overflow: hidden;
  }

  .header::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: linear-gradient(45deg, transparent, rgba(255, 255, 255, 0.1), transparent);
    transform: rotate(45deg);
    animation: shimmer 3s ease-in-out infinite;
  }

  .header-content {
    position: relative;
    z-index: 2;
  }

  .app-title {
    font-size: 2.5rem;
    font-weight: 800;
    color: white;
    margin: 0 0 1rem 0;
    text-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    flex-wrap: wrap;
  }

  .title-icon {
    font-size: 2.2rem;
    animation: pulse 2s ease-in-out infinite;
  }

  .title-accent {
    background: linear-gradient(45deg, #FFD700, #FFA500);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    font-size: 1.5rem;
    font-weight: 900;
  }

  .welcome-section {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    align-items: center;
  }

  .welcome-text {
    font-size: 1.3rem;
    color: rgba(255, 255, 255, 0.95);
    margin: 0;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    flex-wrap: wrap;
    justify-content: center;
  }

  .greeting {
    font-weight: 400;
  }

  .user-name {
    font-weight: 700;
    color: #FFD700;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
  }

  .wave {
    animation: wave 2s ease-in-out infinite;
  }

  .time-display {
    background: rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(10px);
    padding: 0.75rem 1.5rem;
    border-radius: 25px;
    border: 1px solid rgba(255, 255, 255, 0.2);
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .time-icon {
    font-size: 1.2rem;
  }

  .current-time {
    color: white;
    font-weight: 600;
    font-size: 1rem;
    text-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
  }

  @keyframes shimmer {
    0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
    50% { transform: translateX(0%) translateY(0%) rotate(45deg); }
    100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
  }

  @keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.1); }
  }

  @keyframes wave {
    0%, 100% { transform: rotate(0deg); }
    25% { transform: rotate(20deg); }
    75% { transform: rotate(-10deg); }
  }

  @media (max-width: 768px) {
    .header {
      padding: 1.5rem 0.5rem;
      border-radius: 0 0 20px 20px;
    }

    .app-title {
      font-size: 2rem;
    }

    .welcome-text {
      font-size: 1.1rem;
    }

    .time-display {
      padding: 0.5rem 1rem;
    }

    .current-time {
      font-size: 0.9rem;
    }
  }
</style>
