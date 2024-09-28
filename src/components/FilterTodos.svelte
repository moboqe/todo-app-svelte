<script lang="ts">
  import type { FiltersType } from "$root/types/todo";

  type SetFilterType = (newFilter: FiltersType) => void;

  export let selectedFilter: FiltersType;
  export let setFilter: SetFilterType;

  let filters = new Map<FiltersType, string>([
    ["all", "Все"],
    ["active", "Активные"],
    ["completed", "Завершенные"],
  ]);
</script>

<div class="filters">
  <div style="float:left">Фильтры</div>
  <div style="float:left; clear:left">
    {#each filters.keys() as filter}
      <button
        on:click={() => setFilter(filter)}
        class:selected={selectedFilter === filter}
        class="filter"
      >
        {filters.get(filter)}
      </button>
    {/each}
    <!-- <button class="filter" on:click={showAllTodos}>Все</button>
    <button class="filter" on:click={showActiveTodos}>Активные</button>
    <button class="filter" on:click={showCompletedTodos}>Завершенные</button> -->
  </div>
</div>

<style>
  .filters {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-4);
  }

  .filter {
    text-transform: capitalize;
    padding: var(--spacing-4) var(--spacing-8);
    border: 1px solid transparent;
    border-radius: var(--radius-base);
  }

  .filter:hover {
    border: 1px solid var(--color-highlight);
  }

  .selected {
    border-color: var(--color-highlight);
  }
</style>
