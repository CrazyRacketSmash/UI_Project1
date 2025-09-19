<script>
  export let totalDays;
  export let activeDays;
  let percent = (activeDays / totalDays) * 100;
</script>

<div class="progress-wrapper">
  <p>{activeDays} active days / {totalDays} total</p>
  <div class="progress-bar">
    <div class="progress-fill" style="width: {percent}%;"></div>
  </div>
</div>

<style>
  .progress-wrapper {
    text-align: center;
    margin: 1rem 0;
  }
  .progress-bar {
    height: 10px;
    background: #ddd;
    border-radius: 6px;
    overflow: hidden;
    width: 80%;
    margin: 0.5rem auto;
  }
  .progress-fill {
    height: 100%;
    background: #0077ff;
  }
</style>
