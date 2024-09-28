<script lang="ts">
  import "$root/styles/addtodo.css";
  type AddTodoType = (todo: string) => void;
  type ToggleCompletedType = (event: MouseEvent) => void;
  type TododsAmountType = number;

  export let addTodo: AddTodoType;
  export let toggleCompleted: ToggleCompletedType;
  export let todosAmount: TododsAmountType;

  let todo = "";

  function handleSubmit() {
    addTodo(todo);
    todo = "";
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  {#if todosAmount > 0}
    <input
      type="checkbox"
      id="toggle-all"
      class="toggle-all"
      on:click={toggleCompleted}
    />
    <label aria-label="Mark all as complete" for="toggle-all">
      Отметить все как выполненные
    </label>
  {/if}
  <!-- svelte-ignore a11y-autofocus -->
  <!-- TODO add text validation-->
  <input
    id="new-todo"
    class="new-todo"
    placeholder="Что необходимо сделать?"
    type="text"
    autofocus
    bind:value={todo}
  />
</form>
