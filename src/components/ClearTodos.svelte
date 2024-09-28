<script lang="ts">
  import type { FiltersType } from "$root/types/todo";

  type ClearCompletedType = () => void;

  export let clearCompleted: ClearCompletedType;
  export let completedTodos: number;
</script>

<button
  class="clear-completed"
  class:hidden={completedTodos === 0}
  on:click={clearCompleted}>Удалить завершенные</button
>
