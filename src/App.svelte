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

  let hoursSlept = 7; //Sleep
  let sleptWell = false;

  let selectedMoods = []; //Mood
  let imagePreview = "";
  let caption = "";

  let wentToGym = false; // Gym
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
    { key: 'yoga', label: 'Yoga' },
    { key: 'water', label: 'Water Intake' }
  ];
  let activeActivities = ['sleep', 'gym', 'mood'];
  let lastCustomization = null;

  let theme = 'light';
  let lastTheme = null;

  function saveEntry() {
    const newEntry = {
      id: Date.now(),
      date: entryDate,
      mood: selectedMoods.join(", "),
      image: imagePreview,
      caption,
      gym: { didGym: wentToGym, duration: gymDuration }
    };
    entries = [newEntry, ...entries];
    alert(`✅ Entry Saved!`);
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
    {showPrevious ? "📝 New Entry" : "📖 View Previous Entries"}
  </button>
  <button class="toggle-btn" on:click={() => showOverview = true}>
    Show Overview Performance
  </button>
  {#if showPrevious}
    <PreviousEntries {entries} />
  {:else}
    <Progress totalDays={5} activeDays={3} />

  <!-- Conditionally render forms based on active activities -->
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

  {#if activeActivities.includes('energy')}
    <Card title="⚡ Energy">
      <EnergyForm />
    </Card>
  {/if}

  {#if activeActivities.includes('Walking / Running')}
    <Card title="🏃 Walking / Running">
      <ExerciseForm />
    </Card>
  {/if}

  {#if activeActivities.includes('mood')}
    <Card title="😊 Mood">
      <MoodForm bind:selectedMoods />
    </Card>
  {/if}

  {#if activeActivities.includes('yoga')}
    <Card title="🧘 Yoga">
      <Yoga bind:yogaDuration bind:selectedExercises />
    </Card>
  {/if}

  {#if activeActivities.includes('Post about your day')}
    <Card title="📝 Post about your day">
      <PostForm bind:imagePreview bind:caption />
    </Card>
  {/if}

  {#if activeActivities.includes('water')}
    <Card title="💧 Water Intake">
      <WaterIntake bind:waterCups bind:metWaterGoal />
    </Card>
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
</main>
<style>
  main {
    max-width: 700px;
    margin: 0 auto;
    padding: 2rem;
  }
  .toggle-btn {
    background: #eee;
    border: none;
    padding: 0.75rem 1rem;
    border-radius: 8px;
    margin-bottom: 1.5rem;
    cursor: pointer;
  }
  .overlay {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}
  .modal {
    background: #fff;
    padding: 2rem;
    border-radius: 12px;
    min-width: 300px;
    box-shadow: 0 4px 24px rgba(0,0,0,0.2);
  }
</style>