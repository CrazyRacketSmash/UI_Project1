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
  import mockHistory from '../src/data/mockHistory.json';
  import GoalSettings from '../components/GoalSettings.svelte';
  import OverviewPerformance from '../components/OverviewPerformance.svelte';
  import Customization from '../components/Customization.svelte';
  import Theme from '../components/Theme.svelte';
  import Yoga from '../components/Yoga.svelte';
  import WaterIntake from '../components/WaterIntake.svelte';

  let showPrevious = false;
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

  let waterCups = 0;
  let metWaterGoal = false;
  // State for activities and theme
  let showCustomize = false;
  let allActivities = [
    { key: 'sleep', label: 'Sleep' },
    { key: 'gym', label: 'Gym' },
    { key: 'mood', label: 'Mood' },
    { key: 'energy', label: 'Energy' },
    { key: 'exercise', label: 'Exercise' },
    { key: 'post', label: 'Post about your day' },
    { key: 'yoga', label: 'Yoga' },
    { key: 'water', label: 'Water Intake' }
  ];

  let activeActivities = ['sleep', 'gym', 'mood', 'energy', 'exercise', 'post', 'yoga', 'water'];
  let lastCustomization = null;

  let theme = 'light';
  let lastTheme = null;
  let showToast = false;

  function saveEntry() {
    const newEntry = {
      id: Date.now(),
      date: entryDate,
      sleep: { hours: hoursSlept, quality: sleptWell },
      mood: selectedMoods.join(", "),
      image: imagePreview,
      caption,
      gym: { didGym: wentToGym, duration: gymDuration },
      yoga: { duration: yogaDuration, exercises: selectedExercises },
      water: { cups: waterCups, metGoal: metWaterGoal }
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
  <!-- <Theme bind:theme bind:lastTheme /> -->
  <!-- ShowPrevious -->
  <button class="toggle-btn" on:click={() => showPrevious = !showPrevious}>
    {showPrevious ? "New Entry" : "View Previous Entries"}
  </button>
  <button class="toggle-btn" on:click={() => showOverview = true}>
    Show Overview Performance
  </button>
  {#if showPrevious}
    <PreviousEntries {entries} />
  {:else}
    <Progress totalDays={5} activeDays={3} />

  <!-- Render forms based on active activities -->
  <div class="cards-grid">
    {#if activeActivities.includes('sleep')}
      <Card title="😴 Sleep">
        <SleepForm bind:hoursSlept bind:sleptWell />
      </Card>
    {/if}

    {#if activeActivities.includes('gym')}
      <Card title="💪 Gym">
        <GymForm bind:wentToGym bind:gymDuration />
      </Card>
    {/if}

    {#if activeActivities.includes('mood')}
      <Card title="😊 Mood">
        <MoodForm bind:selectedMoods />
      </Card>
    {/if}

    {#if activeActivities.includes('energy')}
      <Card title="⚡ Energy">
        <EnergyForm />
      </Card>
    {/if}

    {#if activeActivities.includes('exercise')}
      <Card title="🏃 Walking / Running">
        <ExerciseForm />
      </Card>
    {/if}

    {#if activeActivities.includes('post')}
      <Card title="📝 Post about your day">
        <PostForm bind:imagePreview bind:caption />
      </Card>
    {/if}

    {#if activeActivities.includes('yoga')}
      <Card title="🧘 Yoga">
        <Yoga bind:yogaDuration bind:selectedExercises />
      </Card>
    {/if}

    {#if activeActivities.includes('water')}
      <Card title="💧 Water Intake">
        <WaterIntake bind:waterCups bind:metWaterGoal />
      </Card>
    {/if}
  </div>
  {#if showToast}
    <div class="toast">
      <div class="toast-content">
        <span class="toast-icon">✅</span>
        <span class="toast-message">Entry Saved Successfully!</span>
        <button class="toast-close" on:click={() => showToast = false}>×</button>
      </div>
    </div>
  {/if}

  <GoalSettings />

  <button class="save-btn" on:click={saveEntry}>💾 Save Health Log</button>
  {/if}

  <!-- Show Overview Modal -->
  {#if showOverview}
    <div class="overlay">
      <div class="modal">
        <OverviewPerformance {entries} />
        <br />
        <button on:click={() => showOverview = false}>OK</button>
      </div>
    </div>
  {/if}

  <!-- Toast Notification -->
  {#if showToast}
    <div class="toast">
      <div class="toast-content">
        <span class="toast-message">Entry Saved Successfully!</span>
        <button class="toast-close" on:click={() => showToast = false}>×</button>
      </div>
    </div>
  {/if}
</main>