<script>
  import PollStore from '../stores/pollStore.js';
  import { createEventDispatcher } from 'svelte';
  import Button from '../shared/Button.svelte';

  let dispatch = createEventDispatcher();

  let fields = { question: '', answerA: '', answerB: '' };
  let errors = { question: '', answerA: '', answerB: '' };
  let valid = false;

  const submitHandler = () => {
    valid = true;
    // question
    if (fields.question.trim().length < 5) {
      valid = false;
      errors.question = 'Question must be at least 5 chars long'
    } else {
      errors.question = ''
    }

    // answer A
    if (fields.answerA.trim().length < 1) {
      valid = false;
      errors.answerA = 'Answer A cannot be empty'
    } else {
      errors.answerA = ''
    }

    // answer B
    if (fields.answerB.trim().length < 1) {
      valid = false;
      errors.answerB = 'Answer B cannot be empty'
    } else {
      errors.answerB = ''
    }
    
    // add new poll
    if (valid) {
      let poll = {...fields, id: Math.random(), votesA: 0, votesB: 0};
      // save poll to store
      PollStore.update(currentPolls => {
        return [poll, ...currentPolls];
      });
      dispatch('add');
    }
  }
</script>

<form on:submit|preventDefault={submitHandler}>
  <div class="form-field">
    <label for="question">Poll Question:</label>
    <input type="text" id="question" bind:value={fields.question}>
    <div class="error">{ errors.question }</div>
  </div>
  <div class="form-field">
    <label for="answer-a">Answer A value:</label>
    <input type="text" id="answer-a" bind:value={fields.answerA}>
    <div class="error">{ errors.answerA }</div>
  </div>
  <div class="form-field">
    <label for="answer-b">Answer B value:</label>
    <input type="text" id="answer-b" bind:value={fields.answerB}>
    <div class="error">{ errors.answerB }</div>
  </div>
  <Button>Add Poll</Button>
</form>

<style>
  form {
    width: 100%;
    max-width: 500px;
    margin: 0 auto;
    padding: 1.5rem;
    border-radius: 0.75rem;
    background-color: white;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05), 0 1px 3px rgba(0, 0, 0, 0.1);
  }
  
  .form-field {
    margin: 1.5rem 0;
    text-align: left;
  }
  
  input {
    width: 100%;
    padding: 0.75rem 1rem;
    border: 1px solid #e2e8f0;
    border-radius: 0.5rem;
    font-size: 1rem;
    transition: all 0.2s ease;
    background-color: #f8fafc;
    box-sizing: border-box;
  }
  
  input:focus {
    outline: none;
    border-color: #4f46e5;
    box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.1);
    background-color: white;
  }
  
  label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 500;
    color: #334155;
    font-size: 0.9rem;
  }
  
  .error {
    margin-top: 0.5rem;
    font-size: 0.8rem;
    color: #e11d48;
    font-weight: 500;
    transition: all 0.2s ease;
  }
  
  /* Styling for Button component - add these if you want to customize the button */
  :global(form button) {
    margin-top: 1rem;
    background-color: #4f46e5;
    color: white;
    font-weight: 500;
    padding: 0.75rem 1.5rem;
    border-radius: 0.5rem;
    transition: background-color 0.2s ease;
  }
  
  :global(form button:hover) {
    background-color: #4338ca;
  }
</style>