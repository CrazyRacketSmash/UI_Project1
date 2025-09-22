<script>
  import Header from '../components/Header.svelte';
  import Progress from '../components/Progress.svelte';
  import Card from '../components/Card.svelte';
  import SleepForm from '../components/SleepForm.svelte';
  import ExerciseForm from '../components/ExerciseForm.svelte';
  import EnergyForm from '../components/EnergyForm.svelte';
  import MoodForm from '../components/Mood.svelte';
  import PostForm from '../components/Post.svelte';
  import GymForm from '../components/Gym.svelte';
  import PreviousEntries from '../components/History.svelte';
  import mockHistory from "../data/mockHistory.json";
  import GoalSettings from '../components/GoalSettings.svelte';
  import OverviewPerformance from '../components/OverviewPerformance.svelte';
  import Customization from '../components/Customization.svelte';
  import Theme from '../components/Theme.svelte';
  import Yoga from '../components/Yoga.svelte';
  import WaterIntake from '../components/WaterIntake.svelte';

  let showHistory = false;
  let showOverview = false;

  let hoursSlept = 7;
  let sleptWell = false;

  let selectedMoods = [];
  let imagePreview = "";
  let caption = "";

  let wentToGym = false;
  let gymDuration = 0;

  let entryDate = new Date().toISOString().split("T")[0];
  let entries = [...mockHistory];

  let yogaDuration = 0;
  let selectedExercises = [];

  let energyLevel = "";
  let tookWalk = false;
  let wentRunning = false;
  let distance = 0;

  let waterCups = 0;
  let metWaterGoal = false;

  // State for activities and theme
  let showCustomize = false;
  let allActivities = [
    { key: 'mood', label: 'Mood' },
    { key: 'energy', label: 'Energy' },
    { key: 'sleep', label: 'Sleep' },
    { key: 'gym', label: 'Gym' },
    { key: 'exercise', label: 'Exercise' },
    { key: 'yoga', label: 'Yoga' },
    { key: 'water', label: 'Water Intake' },
    { key: 'post', label: 'Post about your day' }
  ];

  let activeActivities = ['sleep', 'gym', 'mood', 'energy', 'exercise', 'post', 'yoga', 'water'];
  let lastCustomization = null;

  // let theme = 'green';
  // let lastTheme = null;
  let showToast = false;
  let showGoalSettings = false;
  let manualTotalDays = 10; // Default total days
  let goals = {
    sleep: 7,
    gym: 30,
    yoga: 20,
    water: 8,
    exercise: 1
  };

  $: totalDays = manualTotalDays;
  $: activeDays = entries.length;
  
  function increaseTotalDays() {
    manualTotalDays += 1;
  }
  
  function decreaseTotalDays() {
    if (manualTotalDays > 1) {
      manualTotalDays -= 1;
    }
  }
  
  function saveEntry() {
    const newEntry = {
      id: Date.now(),
      date: entryDate,
      sleep: { hours: hoursSlept},
      mood: selectedMoods.join(", "),
      image: imagePreview,
      caption,
      gym: { didGym: wentToGym, duration: gymDuration },
      yoga: { duration: yogaDuration, exercises: selectedExercises },
      water: { cups: waterCups },
      energy: energyLevel,
      exercise: { tookWalk, wentRunning, distance }
    };
    entries = [newEntry, ...entries];
    showToast = true;
    setTimeout(() => showToast = false, 3000); // Auto-hide after 3 seconds
  }
</script>

<Customization
  bind:showCustomize
  bind:allActivities
  bind:activeActivities
  bind:lastCustomization
/>

<main>
  <Header name="Huy" />

  <div class="nav-bar">
    <button class="toggle-btn" on:click={() => {showHistory = !showHistory; showOverview = false;}}>
      {showHistory ? "New Log" : "History"}
    </button>
    <button class="toggle-btn" on:click={() => {showOverview = !showOverview; showHistory = false;}}>
      {showOverview ? "New Log" : "Overview Performance"}
    </button>
    <button class="toggle-btn" on:click={() => showCustomize = true}>
      Customize Activities
    </button>
    <button class="toggle-btn" on:click={() => showGoalSettings = true}>
      Set Goals
    </button>
  </div>

  {#if showHistory}
    <PreviousEntries {entries} />
  {:else if showOverview}
    <OverviewPerformance {entries} {goals} />
  {:else}
    <Progress {totalDays} {activeDays} onIncreaseTotalDays={increaseTotalDays} onDecreaseTotalDays={decreaseTotalDays} />

  <!-- Render forms -->
  <div class="cards-grid">
    {#if activeActivities.includes('mood')}
      <Card title="Mood">
        <MoodForm bind:selectedMoods />
      </Card>
    {/if}

    {#if activeActivities.includes('energy')}
      <Card title="Energy">
        <EnergyForm bind:energyLevel />
      </Card>
    {/if}

    {#if activeActivities.includes('sleep')}
      <Card title="Sleep" goal={goals.sleep} goalUnit="hours">
        <SleepForm bind:hoursSlept />
      </Card>
    {/if}

    {#if activeActivities.includes('gym')}
      <Card title="Gym" goal={goals.gym} goalUnit="min">
        <GymForm bind:wentToGym bind:gymDuration />
      </Card>
    {/if}

    {#if activeActivities.includes('exercise')}
      <Card title="Cardio" goal={goals.exercise} goalUnit="miles">
        <ExerciseForm bind:tookWalk bind:wentRunning bind:distance />
      </Card>
    {/if}

    {#if activeActivities.includes('yoga')}
      <Card title="Yoga" goal={goals.yoga} goalUnit="min">
        <Yoga bind:yogaDuration bind:selectedExercises />
      </Card>
    {/if}

    {#if activeActivities.includes('water')}
      <Card title="Water Intake" goal={goals.water} goalUnit="cups">
        <WaterIntake bind:waterCups />
      </Card>
    {/if}

    {#if activeActivities.includes('post')}
      <Card title="Post about your day">
        <PostForm bind:imagePreview bind:caption />
      </Card>
    {/if}
  </div>
  {#if showToast}
    <div class="toast">
      <div class="toast-content">
        <span class="toast-message">Entry Saved Successfully!</span>
        <button class="toast-close" on:click={() => showToast = false}>x</button>
      </div>
    </div>
  {/if}

  <button class="save-btn" on:click={saveEntry}>Save Changes</button>
  {/if}
  <!-- Toast Notification -->
  {#if showToast}
    <div class="toast">
      <div class="toast-content">
        <span class="toast-message">Entry Saved Successfully!</span>
        <button class="toast-close" on:click={() => showToast = false}>x</button>
      </div>
    </div>
  {/if}

  <GoalSettings bind:showGoalSettings bind:goals />
  <!-- <Theme bind:theme bind:lastTheme /> -->
</main>