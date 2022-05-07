<script>
  // components
  import Title from './Title.svelte';

  // variables
  export let hideForm;
  export let name = '';
  export let amount = null;
  export let addExpense;
  export let isEditingItem;
  export let editExpenseToUI;

  // reactive variables
  $: isInputFieldsEmpty = !name || !amount; 

  // functions
  const handleSubmit = () => {
    
    if(isEditingItem) {
      editExpenseToUI({ name, amount });
    } else {
      addExpense({ name, amount });
    }

    name = '';
    amount = null;

  };
</script>

<section class="form">
  <Title>
    {#if isEditingItem} Edit Expense {:else} Add Expense {/if}
  </Title>
  <form class="expense-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <label for="name">item name</label>
      <input type="text" id="name" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="amount">amount</label>
      <input type="number" id="amount" bind:value={amount} />
    </div>

    <button
      type="submit"
      class="btn btn-block"
      class:disabled={isInputFieldsEmpty}
      disabled={isInputFieldsEmpty}
    >
      {#if isEditingItem} edit expense {:else} add expense {/if}
    </button>

    <button type="button" class="close-btn" on:click={hideForm}>
      <i class="fas fa-times" />
      close
    </button>
  </form>
</section>
