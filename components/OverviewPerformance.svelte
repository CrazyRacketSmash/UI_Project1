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

<style>
    table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 1rem;
    }
    th, td {
        border: 1px solid #ddd;
        padding: 0.5rem 0.75rem;
    }
    th {
        background: #f5f5f5;
    }
    .center {
        text-align: center;
        vertical-align: middle;
    }
</style>