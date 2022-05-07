<script>
  // components
  import { fly } from 'svelte/transition';
  import { flip } from 'svelte/animate';

  import SectionTitle from './Title.svelte';
  import SingleExpense from './SingleExpense.svelte';

  // variables
  export let expenses = [];

</script>

<section>
  <SectionTitle title="expense list" />
  <ul>
    {#each expenses as expense, index (expense.id)}
      <div transition:fly={{x:200, delay:index*300}} animate:flip>
        <!-- 1st item delay = 0 * 300sec, 2nd item delay = 1 * 300sec, 3rd item delay = 2 * 300sec where 0, 1,
             2 are the indices of the items. -->

        <SingleExpense {...expense} />
      </div>
    {:else}
      <h2>Currently you have no expenses. Please add one.</h2>
    {/each}
  </ul>
</section>

<style>
  h2 {
    text-transform: uppercase;
  }
</style>
