<script>
  export let entries = [];
  export let goals = { sleep: 7, gym: 30 };

  // For each entry, determine if the goal was met
  $: sleepGoalDays = entries.map(e => ({
    date: e.date,
    hours: e.sleep?.hours ?? 0,
    met: (e.sleep?.hours ?? 0) >= goals.sleep
  }));

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
  $: yogaFreq = entries.filter(e => e.yoga?.duration > 0).length;
  $: yogaFreqRate = entries.length ? Math.round((yogaFreq / entries.length) * 100) : 0;

  // Water stats  
  $: avgWater = entries.length
    ? (entries.reduce((sum, e) => sum + (e.water?.cups || 0), 0) / entries.length).toFixed(1)
    : 0;
  $: waterGoalMet = entries.filter(e => e.water?.metGoal).length;
  $: waterGoalRate = entries.length ? Math.round((waterGoalMet / entries.length) * 100) : 0;
</script>

<div class="overview">
  <h2>Overview</h2>
  <div>
    <h3>Sleep</h3>
    <p>Average: {avgSleep} hours</p>
    <p>Goal: {goals.sleep} hours</p>
    <p>Goal Met: {sleepGoalRate}% of days</p>
  </div>
  <div>
    <h3>Gym</h3>
    <p>Average: {avgGym} min</p>
    <p>Goal: {goals.gym} min</p>
    <p>Goal Met: {gymGoalRate}% of days</p>
  </div>
  <div>
    <h3>Yoga</h3>
    <p>Average: {avgYoga} min</p>
    <p>Frequency: {yogaFreqRate}% of days</p>
  </div>
  <div>
    <h3>Water Intake</h3>
    <p>Average: {avgWater} cups</p>
    <p>Goal Met: {waterGoalRate}% of days</p>
  </div>
  <h3>Sleep Goal Achievement</h3>
  <table>
    <thead>
      <tr>
        <th>Date</th>
        <th>Hours Slept</th>
        <th>Goal Met?</th>
      </tr>
    </thead>
    <tbody>
      {#each sleepGoalDays as day}
        <tr>
          <td>{day.date}</td>
          <td class="center">{day.hours}</td>
          <td class="center">{day.met ? '✅' : '❌'}</td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>

