<script>
  import { setContext, onMount, afterUpdate } from "svelte";
  // test
  // state
  // all context variables & functions more than one level down included in state object
  const state = {
    removeExpense: removeExpense,
    setModifiedExpense: setModifiedExpense
  };
  // context
  setContext("state", state);

  // components
  import Navbar from "./Navbar.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  import Modal from "./Modal.svelte";

  // data
  // This was later replaced by saving [expenses] into localStorage
  // import expensesData from "./expenses";

  // variables
  let expenses = [];
  // set editing variables
  let setId = null;
  let setName = "";
  let setAmount = null;
  // toggle form variables
  let isFormOpen = false;
  // reactive variables
  $: total = expenses.reduce((acc, curr) => {
    return (acc += curr.amount);
  }, 0);
  $: isEditing = setId ? true : false;

  // functions
  function showForm() {
    isFormOpen = true;
  }
  function hideForm() {
    isFormOpen = false;
    setId = null;
    setName = "";
    setAmount = null;
  }

  // Include this function in state to pass to Expense component
  function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
  }

   // this function is also set in state to pass to Expense component
  function setModifiedExpense(id) {
    let expense = expenses.find(item => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  }

  function addExpense({ name, amount }) {
    let expense = {
      id: Math.random() * Date.now(),
      name,
      amount
    };
    expenses = [expense, ...expenses];
  }
 
  function editExpense({ name, amount }) {
    expenses = expenses.map(item => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });
    setId = null;
    setName = "";
    setAmount = null;
  }

  function clearExpenses() {
    expenses = [];
  }

  // local storage
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }
  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });
  // afterUpdate not a good idea if it runs a time-consuming process
  // if time-consuming, split the time-consuming part into another function
  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<Navbar {showForm} />
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {addExpense}
        name={setName}
        amount={setAmount}
        {isEditing}
        {editExpense}
        {hideForm} />
    </Modal>
  {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>
    clear expenses
  </button>
</main>
