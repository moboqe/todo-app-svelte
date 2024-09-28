<script lang="ts">
  import "$root/styles/todo.css";
  import { fade, slide } from "svelte/transition";
  import type { ITodo } from "$root/types/todo";

  type CompleteTodoType = (id: string) => void;
  type RemoveTodoType = (id: string) => void;
  type EditTodoType = (id: string, newTodo: string) => void;

  export let todo: ITodo;
  export let completeTodo: CompleteTodoType;
  export let removeTodo: RemoveTodoType;
  export let editTodo: EditTodoType;

  let editing: boolean = false;
  function toggleEdit(): void {
    editing = true;
  }
  function handleEdit(event: KeyboardEvent, id: string): void {
    let targetElement = event.target as HTMLInputElement;
    switch (event.key) {
      case "Escape":
        targetElement.blur();
        break;
      case "Enter":
        editTodo(id, targetElement.value);
        targetElement.blur();
        break;
      default:
        break;
    }
  }
  function handleBlur(event: FocusEvent, id: string): void {
    let targetElement = event.target as HTMLInputElement;
    let newTodo = targetElement.value;
    editTodo(id, newTodo);
    targetElement.blur();
    editing = false;
  }
</script>

<li in:slide out:fade class:editing class="todo">
  <div class="todo-item">
    <div>
      <input
        checked={todo.completed}
        id="todo"
        class="toggle"
        type="checkbox"
        on:change={() => completeTodo(todo.id)}
      />
      <label aria-label="Check todo" class="todo-check" for="todo" />
    </div>
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <span
      class="todo-text"
      class:completed={todo.completed}
      on:dblclick={toggleEdit}>{todo.text}</span
    >
    <button
      aria-label="Remove todo"
      class="remove"
      on:click={() => removeTodo(todo.id)}
    />
  </div>
  {#if editing}
    <!-- svelte-ignore a11y-autofocus -->
    <input
      class="edit"
      type="text"
      value={todo.text}
      autofocus
      on:keydown={(e) => handleEdit(e, todo.id)}
      on:blur={(e) => handleBlur(e, todo.id)}
    />
  {/if}
</li>
