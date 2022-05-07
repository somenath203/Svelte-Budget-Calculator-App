<script>

  // getContext
  import {getContext} from 'svelte';
  import {fly} from 'svelte/transition';
  import {quintOut} from 'svelte/easing';

  // datas
  export let id;
  export let name = '';
  export let amount = 0;

  let displayAmountOnCLick = false;
  
  const toggleAmount = () => {
    displayAmountOnCLick = !displayAmountOnCLick;
  };
  
  const removeExpense = getContext('remove');
  const setEditedExpense = getContext('edited');

</script>

<article class="single-expense">
  <div class="expense-info">

    <h2>
      {name}
      <button class="amount-btn" on:click={toggleAmount}>
        <i class="fas fa-caret-down" />
      </button>
    </h2>

    {#if displayAmountOnCLick}
      <h4 transition:fly={{x: 100, y: 100, duration: 1000, delay: 500, easing: quintOut}}> 
        <!-- 500 means half a second -->

        Amount: Rs.{amount}

      </h4>
    {/if}

  </div>

  <div class="expense-button">
    <button class="expense-btn edit-btn" on:click={() => {setEditedExpense(id)}}>
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn" on:click={() => removeExpense(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
  
</article>
