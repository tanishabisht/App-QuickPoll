<script>
	import Header from './components/Header.svelte';
	import PollList from './components/PollList.svelte';
</script>

<Header />
<main>
  <PollList />
</main>

<style>
	main{
    width: 100%;
    max-width: 960px;
    margin: 40px auto;
  }
</style>