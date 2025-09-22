<script>
  export let entries = [];
  export let goals = { sleep: 7, gym: 30, yoga: 20, water: 8 };

  // Sleep stats
  $: avgSleep = entries.length
    ? (entries.reduce((sum, e) => sum + (e.sleep?.hours || 0), 0) / entries.length).toFixed(1)
    : 0;
  $: sleepGoalMet = entries.filter(e => (e.sleep?.hours || 0) >= goals.sleep).length;
  $: sleepGoalRate = entries.length ? Math.round((sleepGoalMet / entries.length) * 100) : 0;

  // Gym stats
  $: avgGym = entries.length
    ? (entries.reduce((sum, e) => sum + (e.gym?.duration || 0), 0) / entries.length).toFixed(1)
    : 0;
  $: gymGoalMet = entries.filter(e => (e.gym?.didGym && e.gym.duration >= goals.gym)).length;
  $: gymGoalRate = entries.length ? Math.round((gymGoalMet / entries.length) * 100) : 0;

  // Yoga stats
  $: avgYoga = entries.length
    ? (entries.reduce((sum, e) => sum + (e.yoga?.duration || 0), 0) / entries.length).toFixed(1)
    : 0;
  $: yogaGoalMet = entries.filter(e => (e.yoga?.duration || 0) >= goals.yoga).length;
  $: yogaGoalRate = entries.length ? Math.round((yogaGoalMet / entries.length) * 100) : 0;

  // Water stats  
  $: avgWater = entries.length
    ? (entries.reduce((sum, e) => sum + (e.water?.cups || 0), 0) / entries.length).toFixed(1)
    : 0;
  $: waterGoalMet = entries.filter(e => (e.water?.cups || 0) >= goals.water).length;
  $: waterGoalRate = entries.length ? Math.round((waterGoalMet / entries.length) * 100) : 0;

  // Mood stats
  $: moodLogged = entries.filter(e => e.mood && e.mood.length > 0).length;
  $: moodLogRate = entries.length ? Math.round((moodLogged / entries.length) * 100) : 0;

  // Energy stats
  $: energyLogged = entries.filter(e => e.energy !== undefined).length;
  $: energyLogRate = entries.length ? Math.round((energyLogged / entries.length) * 100) : 0;

  // Exercise stats
  $: exerciseLogged = entries.filter(e => e.exercise !== undefined).length;
  $: exerciseLogRate = entries.length ? Math.round((exerciseLogged / entries.length) * 100) : 0;

  // Post stats
  $: postLogged = entries.filter(e => e.caption && e.caption.length > 0).length;
  $: postLogRate = entries.length ? Math.round((postLogged / entries.length) * 100) : 0;

  // Comprehensive goal achievement data for table
  $: goalAchievementData = entries.map(entry => ({
    date: entry.date,
    sleep: {
      value: entry.sleep?.hours ?? 0,
      goal: goals.sleep,
      met: (entry.sleep?.hours ?? 0) >= goals.sleep
    },
    gym: {
      value: entry.gym?.duration ?? 0,
      goal: goals.gym,
      met: entry.gym?.didGym && (entry.gym?.duration ?? 0) >= goals.gym
    },
    yoga: {
      value: entry.yoga?.duration ?? 0,
      goal: goals.yoga,
      met: (entry.yoga?.duration ?? 0) >= goals.yoga
    },
    water: {
      value: entry.water?.cups ?? 0,
      goal: goals.water,
      met: (entry.water?.cups ?? 0) >= goals.water
    }
  }));
</script>

<div class="overview-stats">  
  <div class="stat-grid">
    <div class="stat-card">
      <h3>Sleep</h3>
      <div class="stat-info">
        <p><span class="stat-value">{avgSleep}h</span> average</p>
        <p><span class="stat-rate">{sleepGoalRate}%</span> goal met</p>
      </div>
    </div>

    <div class="stat-card">
      <h3>Gym</h3>
      <div class="stat-info">
        <p><span class="stat-value">{avgGym}min</span> average</p>
        <p><span class="stat-rate">{gymGoalRate}%</span> goal met</p>
      </div>
    </div>

    <div class="stat-card">
      <h3>Yoga</h3>
      <div class="stat-info">
        <p><span class="stat-value">{avgYoga}min</span> average</p>
        <p><span class="stat-rate">{yogaGoalRate}%</span> goal met</p>
      </div>
    </div>

    <div class="stat-card">
      <h3>Water</h3>
      <div class="stat-info">
        <p><span class="stat-value">{avgWater}</span> cups average</p>
        <p><span class="stat-rate">{waterGoalRate}%</span> goal met</p>
      </div>
    </div>

    <div class="stat-card">
      <h3>Mood</h3>
      <div class="stat-info">
        <p><span class="stat-value">{moodLogged}</span> entries</p>
        <p><span class="stat-rate">{moodLogRate}%</span> logged</p>
      </div>
    </div>

    <div class="stat-card">
      <h3>Energy</h3>
      <div class="stat-info">
        <p><span class="stat-value">{energyLogged}</span> entries</p>
        <p><span class="stat-rate">{energyLogRate}%</span> logged</p>
      </div>
    </div>

    <div class="stat-card">
      <h3>Exercise</h3>
      <div class="stat-info">
        <p><span class="stat-value">{exerciseLogged}</span> entries</p>
        <p><span class="stat-rate">{exerciseLogRate}%</span> logged</p>
      </div>
    </div>

    <div class="stat-card">
      <h3>Post</h3>
      <div class="stat-info">
        <p><span class="stat-value">{postLogged}</span> entries</p>
        <p><span class="stat-rate">{postLogRate}%</span> logged</p>
      </div>
    </div>
  </div>

  {#if entries.length > 0}
    <div class="goal-achievement-table">
      <h3>Goal Achievement Tracker</h3>
      <div class="table-wrapper">
        <table>
          <thead>
            <tr>
              <th>Date</th>
              <th>Sleep Target: {goals.sleep}h</th>
              <th>Gym Target: {goals.gym}min</th>
              <th>Yoga Target: {goals.yoga}min</th>
              <th>Water Target: {goals.water} cups</th>
            </tr>
          </thead>
          <tbody>
            {#each goalAchievementData as day}
              <tr>
                <td class="date-cell">{day.date}</td>
                <td class="stat-cell {day.sleep.met ? 'goal-met' : 'goal-missed'}">
                  <div class="cell-content">
                    <span class="value">{day.sleep.value}h</span>
                    <span class="status {day.sleep.met ? 'achieved' : 'missed'}">{day.sleep.met ? 'Achieved' : 'Missed'}</span>
                  </div>
                </td>
                <td class="stat-cell {day.gym.met ? 'goal-met' : 'goal-missed'}">
                  <div class="cell-content">
                    <span class="value">{day.gym.value}min</span>
                    <span class="status {day.gym.met ? 'achieved' : 'missed'}">{day.gym.met ? 'Achieved' : 'Missed'}</span>
                  </div>
                </td>
                <td class="stat-cell {day.yoga.met ? 'goal-met' : 'goal-missed'}">
                  <div class="cell-content">
                    <span class="value">{day.yoga.value}min</span>
                    <span class="status {day.yoga.met ? 'achieved' : 'missed'}">{day.yoga.met ? 'Achieved' : 'Missed'}</span>
                  </div>
                </td>
                <td class="stat-cell {day.water.met ? 'goal-met' : 'goal-missed'}">
                  <div class="cell-content">
                    <span class="value">{day.water.value} cups</span>
                    <span class="status {day.water.met ? 'achieved' : 'missed'}">{day.water.met ? 'Achieved' : 'Missed'}</span>
                  </div>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
    </div>
  {/if}
</div>

<style>
  .overview-stats {
    padding: 2rem;
    max-width: 1200px;
    margin: 0 auto;
  }

  .stat-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1.5rem;
    margin-bottom: 3rem;
  }

  .stat-card {
    background: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(76, 175, 80, 0.2);
    border-radius: 16px;
    padding: 1.5rem;
    text-align: center;
    box-shadow: 0 8px 32px rgba(76, 175, 80, 0.1);
    transition: all 0.3s ease;
  }

  .stat-card:hover {
    border-color: var(--green-primary);
  }

  .stat-card h3 {
    font-size: 1.2rem;
    margin-bottom: 1rem;
    color: var(--green-primary);
    font-weight: 600;
  }

  .stat-info {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .stat-value {
    font-size: 1.8rem;
    font-weight: bold;
    color: var(--green-dark);
  }

  .stat-rate {
    font-size: 1.2rem;
    font-weight: 600;
    color: var(--green-primary);
  }

  .goal-achievement-table {
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    border-radius: 20px;
    padding: 2rem;
    box-shadow: 0 8px 32px rgba(76, 175, 80, 0.1);
    border: 1px solid rgba(76, 175, 80, 0.2);
  }

  .goal-achievement-table h3 {
    text-align: center;
    font-size: 1.5rem;
    margin-bottom: 1.5rem;
    color: var(--green-primary);
    font-weight: 700;
    letter-spacing: 0.5px;
  }

  .table-wrapper {
    overflow-x: auto;
    border-radius: 12px;
    box-shadow: 0 4px 20px rgba(76, 175, 80, 0.1);
    border: 1px solid rgba(76, 175, 80, 0.15);
  }

  table {
    width: 100%;
    border-collapse: collapse;
    background: white;
    border-radius: 12px;
    overflow: hidden;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }

  th {
    background: linear-gradient(135deg, var(--green-primary), var(--green-secondary));
    color: white;
    padding: 1.2rem 1rem;
    text-align: center;
    font-weight: 600;
    font-size: 0.95rem;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    border-bottom: 2px solid rgba(255, 255, 255, 0.2);
  }

  td {
    padding: 1rem;
    text-align: center;
    border-bottom: 1px solid rgba(76, 175, 80, 0.08);
    vertical-align: middle;
  }

  .date-cell {
    font-weight: 700;
    color: var(--green-dark);
    background: rgba(76, 175, 80, 0.03);
    font-size: 0.95rem;
    letter-spacing: 0.3px;
  }

  .stat-cell {
    position: relative;
    font-weight: 500;
    padding: 0.8rem;
  }

  .cell-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.3rem;
  }

  .value {
    font-size: 1.1rem;
    font-weight: 700;
    color: #2c3e50;
  }

  .status {
    font-size: 0.8rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    padding: 0.2rem 0.6rem;
    border-radius: 12px;
    min-width: 70px;
  }

  .status.achieved {
    background: #e8f5e8;
    color: #2e7d32;
    border: 1px solid #4caf50;
  }

  .status.missed {
    background: #ffebee;
    color: #c62828;
    border: 1px solid #f44336;
  }

  .goal-met {
    background: rgba(76, 175, 80, 0.04);
  }

  .goal-missed {
    background: rgba(244, 67, 54, 0.04);
  }

  tr:hover {
    background: rgba(76, 175, 80, 0.05);
  }

  @media (max-width: 768px) {
    .overview-stats {
      padding: 1rem;
    }

    .stat-grid {
      grid-template-columns: 1fr;
      gap: 1rem;
    }

    .goal-achievement-table {
      padding: 1rem;
    }

    table {
      font-size: 0.85rem;
    }

    th, td {
      padding: 0.6rem 0.4rem;
    }
  }
</style>