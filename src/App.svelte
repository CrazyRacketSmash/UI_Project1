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

<main>
  <Header name="Huy" />
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

  <Card title="😴 Sleep">
    <SleepForm bind:hoursSlept bind:sleptWell />
  </Card>

  <Card title="🏃 Walking / Running">
    <ExerciseForm />
  </Card>

  <Card title="⚡ Energy">
    <EnergyForm />
  </Card>

  <Card title="😊 Mood">
    <MoodForm bind:selectedMoods />
  </Card>

  <Card title="📝 Post about your day">
    <PostForm bind:imagePreview bind:caption />
  </Card>

  <Card title="💪 Gym">
    <GymForm bind:wentToGym bind:gymDuration />
  </Card>

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