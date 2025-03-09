<script>
  import PollStore from '../stores/pollStore.js';
  import Card from '../shared/Card.svelte';
  import Button from '../shared/Button.svelte';
  import { tweened } from 'svelte/motion';

  export let poll;

  // reactive values
  $: totalVotes = poll.votesA + poll.votesB;
  $: percentA = Math.floor(100 / totalVotes * poll.votesA) || 0;
  $: percentB = Math.floor(100 / totalVotes * poll.votesB) || 0;

  // tweened percentages
  const tweenedA = tweened(0);
  const tweenedB = tweened(0);
  $: tweenedA.set(percentA);
  $: tweenedB.set(percentB);
  $: console.log($tweenedA, $tweenedB);

  // handling votes
  const handleVote = (option, id) => {
    PollStore.update(currentPolls => {

      let copiedPolls = [...currentPolls];
      let upvotedPoll = copiedPolls.find(poll => poll.id == id);

      if(option === 'a'){
        upvotedPoll.votesA++;
      }
      if(option === 'b'){
        upvotedPoll.votesB++;
      }

      return copiedPolls;
    });
  };

  // deleting a poll
  const handleDelete = (id) => {
    PollStore.update(polls => {
      return polls.filter(poll => poll.id != id);
    });
  };
</script>

<Card>
  <div class="poll">
    <h3>{ poll.question }</h3>
    <p>Total votes: { totalVotes }</p>
    <div class="answer" on:click={() => handleVote('a', poll.id)}>
      <div class="percent percent-a" style="width: {$tweenedA}%"></div>
      <span data-percent={$tweenedA}>{ poll.answerA } ({ poll.votesA } votes)</span>
    </div>
    <div class="answer" on:click={() => handleVote('b', poll.id)}>
      <div class="percent percent-b" style="width: {$tweenedB}%"></div>
      <span data-percent={$tweenedB}>{ poll.answerB } ({ poll.votesB } votes)</span>
    </div>
    <div class="delete">
      <Button flat={true} on:click={() => handleDelete(poll.id)}>Delete</Button>
    </div>
  </div>
</Card>

<style>
  h3 {
    margin: 0;
    font-size: 1.25rem;
    font-weight: 600;
    color: #2d3748;
  }
  
  p {
    margin-top: 0.5rem;
    font-size: 0.875rem;
    color: #718096;
    margin-bottom: 1.5rem;
  }
  
  .answer {
    background: #f7fafc;
    border-radius: 0.5rem;
    cursor: pointer;
    margin: 1rem 0;
    position: relative;
    overflow: hidden;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  }
  
  .answer:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  span {
    display: flex;
    justify-content: space-between;
    padding: 0.875rem 1.25rem;
    position: relative;
    z-index: 1;
    font-weight: 500;
  }
  
  span::after {
    content: attr(data-percent) + "%";
    font-weight: bold;
  }
  
  .percent {
    height: 100%;
    position: absolute;
    box-sizing: border-box;
    transition: width 0.8s cubic-bezier(0.16, 1, 0.3, 1);
  }
  
  .percent-a {
    background: rgba(66, 153, 225, 0.15);
    border-left: 4px solid #4299e1;
  }
  
  .percent-b {
    background: rgba(72, 187, 120, 0.15);
    border-left: 4px solid #48bb78;
  }
  
  .delete {
    margin-top: 1.5rem;
    display: flex;
    justify-content: flex-end;
  }
  
  /* Add this if you want to customize the Button component appearance */
  :global(.poll .delete button) {
    font-size: 0.875rem;
    padding: 0.5rem 1rem;
    border-radius: 0.375rem;
    transition: background-color 0.2s ease;
    color: #e53e3e;
    background: rgba(229, 62, 62, 0.1);
  }
  
  :global(.poll .delete button:hover) {
    background: rgba(229, 62, 62, 0.2);
  }
</style>