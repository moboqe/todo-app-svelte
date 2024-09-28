<script lang="ts">
  import "$root/styles/todos.css";
  import { tick } from "svelte";
  import { Guid } from "guid-typescript";
  import type { ITodo, FiltersType } from "$root/types/todo";
  import { useStorage } from "$root/stores/useStorage";
  import AddTodo from "./AddTodo.svelte";
  import Todo from "./Todo.svelte";
  import TodosLeft from "./TodosLeft.svelte";
  import FilterTodos from "./FilterTodos.svelte";
  import ClearTodos from "./ClearTodos.svelte";

  let todos = useStorage<ITodo[]>("todos[]", []);

  $: todosAmount = $todos.length;
  $: filteredTodos = filterTodos($todos, selectedFilter);
  $: completedTodos = $todos.filter((todo) => todo.completed).length;
  $: incompletedTodos = $todos.length - completedTodos;
  $: duration = filtering ? 0 : 250;

  let selectedFilter: FiltersType = "all";
  let filtering: boolean = false;

  function generateRandomGuid(): string {
    return Guid.raw();
  }

  function addTodo(todo: string): void {
    let newTodoItem: ITodo = {
      id: generateRandomGuid(),
      text: todo,
      completed: false,
    };
    $todos = [...$todos, newTodoItem];
  }
  function toggleCompleted(event: MouseEvent): void {
    let { checked } = event.target as HTMLInputElement;
    $todos = $todos.map((todo) => ({
      ...todo,
      completed: checked,
    }));
  }

  function completeTodo(id: string): void {
    $todos = $todos.map((todo) => {
      if (todo.id === id) {
        todo.completed = !todo.completed;
      }
      return todo;
    });
  }

  function clearCompleted(): void {
    $todos = $todos.filter((todo: ITodo) => !todo.completed);
  }

  function removeTodo(id: string): void {
    $todos = $todos.filter((todo: ITodo) => todo.id !== id);
  }

  function editTodo(id: string, newTodo: string): void {
    let currentTodoIndex = $todos.findIndex((todo: ITodo) => todo.id === id);
    $todos[currentTodoIndex].text = newTodo;
  }

  async function setFilter(newFilter: FiltersType): Promise<void> {
    filtering = true;
    await tick();
    selectedFilter = newFilter;
    await tick();
    filtering = false;
  }

  function filterTodos(todos: ITodo[], filter: FiltersType): ITodo[] {
    switch (filter) {
      case "all":
        return todos;
      case "active":
        return todos.filter((todo: ITodo) => !todo.completed);
      case "completed":
        return todos.filter((todo: ITodo) => todo.completed);
      default:
        return todos;
    }
  }
</script>

<main>
  <h1 class="title">todos ;</h1>

  <section class="todos">
    <AddTodo {addTodo} {toggleCompleted} {todosAmount} />

    {#if todosAmount}
      <ul class="todo-list">
        {#each filteredTodos as todo (todo.id)}
          <Todo {todo} {completeTodo} {removeTodo} {editTodo} />
        {/each}
      </ul>

      <div class="actions">
        <TodosLeft {incompletedTodos} />
        <FilterTodos {selectedFilter} {setFilter} />
        <ClearTodos {clearCompleted} {completedTodos} />
      </div>
    {/if}
  </section>
</main>
