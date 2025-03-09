<script>
  import PollStore from '../stores/pollStore.js';
</script>

<div class="poll-list">
  {#each $PollStore as poll (poll.id)}  
    <div>
      <h1>{poll.question}</h1>
      <p>{poll.answerA}</p>
      <p>{poll.answerB}</p>
      <p>{poll.votesA}</p>
      <p>{poll.votesB}</p>
    </div>
  {/each}
</div>

<style>
  .poll-list{
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 20px;
  }
</style>