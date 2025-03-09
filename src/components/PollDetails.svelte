<script>
  export let poll;
</script>

<div>
  <h1>{poll.question}</h1>
  <p>{poll.answerA}</p>
  <p>{poll.answerB}</p>
  <p>{poll.votesA}</p>
  <p>{poll.votesB}</p>
</div>

<style>
  p{
    margin-top: 6px;
    font-size: 14px;
    color: #aaa;
    margin-bottom: 30px;
  }
</style>