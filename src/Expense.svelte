<script>
  import { blur, slide, scale, fade, fly } from "svelte/transition";

  import { getContext } from "svelte";

  // context
  // destructure key: value pairs from the state object coming from App
  const { removeExpense, setModifiedExpense } = getContext("state");

  // props
  export let id;
  export let name = "";
  export let amount = 0;

  // local variables
  let displayAmount = false;
</script>

<article class="single-expense">
  <div class="expense-info">
    <h2>
      {name}
      <!-- use callback to prevent clicked function running when component created-->
      <button
        class="amount-btn"
        on:click={() => {
          displayAmount = !displayAmount;
        }}>
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount}
      <!-- <h4 transition:blur>amount : ${amount}</h4> -->
      <h4 transition:slide>amount : ${amount}</h4>
      <!-- <h4 transition:scale>amount : ${amount}</h4> -->
      <!-- <h4 transition:fade>amount : ${amount}</h4> -->
      <!-- <h4 transition:fly={{x:100, y:100, duration:2000, delay:500}}>amount : ${amount}</h4> -->
    {/if}
  </div>
  <div class="expense-buttons">
    <button
      class="expense-btn edit-btn"
      on:click={() => setModifiedExpense(id)}>
      <i class="fas fa-pen" />
    </button>
    <!-- use callback to prevent clicked function running when component created-->
    <button class="expense-btn delete-btn" on:click={() => removeExpense(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>
