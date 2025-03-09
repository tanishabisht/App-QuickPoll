<script>
	import Header from './components/Header.svelte';
	import PollList from './components/PollList.svelte';
	import CreatePollForm from './components/CreatePollForm.svelte';
  import Tabs from './shared/Tabs.svelte';

  // tabs
  let items = ['Current Polls', 'Add New Poll'];
  let activeItem = 'Current Polls';

  const handleTabChange = (e) => activeItem = e.detail;

  const handleAdd = () => {
    activeItem = 'Current Polls';
  }
</script>

<Header />
<main>
  <Tabs {activeItem} {items} on:tabChange={handleTabChange} />
  {#if activeItem === 'Current Polls'}
    <PollList />
  {:else if activeItem === 'Add New Poll'}
    <CreatePollForm on:add={handleAdd} />
  {/if}
</main>

<style>
	main{
    width: 100%;
    max-width: 960px;
    margin: 40px auto;
  }
</style>