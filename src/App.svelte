<!-- JAVASCRIPT -->
<script>
  // setContext
  import { setContext, onMount } from 'svelte';

  // components
  import Navbar from './Navbar.svelte';
  import ExpenseList from './ExpensesList.svelte';
  import Totals from './Totals.svelte';
  import ExpenseForm from './ExpenseForm.svelte';
  import Modal from './Modal.svelte';

  // variables
  let expenses = [];

  // setting edit-item variable
  let setId = null;
  let setName = '';
  let setAmount = null;

  // toggle open-close-form variables
  let isFormOpen = false;

  // reactive variable
  $: isEditingItem = setId ? true : false;
  $: totalExpense = expenses.reduce((acc, curr) => {
    return (acc = acc + curr.amount);
  }, 0); // 0 is the initial value

  // functions
  const setLocalStorage = () => {
    localStorage.setItem('expenses', JSON.stringify(expenses));
  };

  onMount(() => {
    expenses = localStorage.getItem('expenses')
      ? JSON.parse(localStorage.getItem('expenses'))
      : [];
  });

  const showForm = () => {
    isFormOpen = true;
  };
  const hideForm = () => {
    isFormOpen = false;

    setName = '';
    setAmount = null;
    setId = null;
  };

  const addExpense = ({ name, amount }) => {
    let expense = {
      id: Math.random() * Date.now(),
      name: name,
      amount: amount,
    };

    expenses = [...expenses, expense];

    hideForm();

    setLocalStorage();
  };

  const editParticularExpense = (id) => {
    let expense = expenses.find((item) => item.id === id);

    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;

    showForm();
  };

  const editExpenseToUI = ({ name, amount }) => {
    expenses = expenses.map((item) => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });

    setId = null;
    setAmount = null;
    setName = '';

    hideForm();

    setLocalStorage();
  };

  const removeExpense = (id) => {
    expenses = expenses.filter((item) => item.id !== id);

    setLocalStorage();
  };

  const clearAllExpenses = () => {
    expenses = [];

    setLocalStorage();
  };

  // using setContext
  setContext('remove', removeExpense);
  setContext('edited', editParticularExpense);
  
</script>

<Navbar {showForm} />
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {hideForm}
        {addExpense}
        name={setName}
        amount={setAmount}
        {isEditingItem}
        {editExpenseToUI}
      />
    </Modal>
  {/if}
  <Totals title="total expenses" total={totalExpense} />
  <ExpenseList {expenses} />

  {#if expenses.length > 0}
    <button
      type="button"
      class="btn btn-primary btn-block"
      on:click={clearAllExpenses}>clear all expenses</button
    >
  {/if}
</main>


