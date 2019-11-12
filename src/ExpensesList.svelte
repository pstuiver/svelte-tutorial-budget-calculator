<script>
  import { fly } from "svelte/transition";
  import { flip } from "svelte/animate";
  import Title from "./Title.svelte";
  import Expense from "./Expense.svelte";
  export let expenses = [];
</script>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>

<section>
  <Title title="expense list" />
  <ul>
    {#each expenses as expense, index (expense.id)}
      <div in:fly={{ x: 200, delay: index * 500 }} out:fly={{ x: -200 }} animate:flip>
        <Expense {...expense} />
      </div>
    {:else}
      <h2>you have not added any expenses</h2>
    {/each}
  </ul>
</section>
