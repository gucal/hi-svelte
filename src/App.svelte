<script>
  import { onMount } from "svelte";
  import { v4 as uuidv4 } from "uuid";

  import Todo from "./components/Todo.svelte";

  let todoText = "";
  let todos = [];

  onMount(() => {
    let oldTodos = JSON.parse(localStorage.getItem("todos"));
    if (oldTodos) {
      todos = oldTodos;
    }
  });

  onMount(() => {
    window.addEventListener(
      "beforeunload",
      function (e) {
        localStorage.setItem("todos", JSON.stringify(todos));
      },
      false
    );
  });

  const addTodo = () => {
    if (todoText.trim().length < 1) {
      alert("Metin kutusu boş bırakılamaz!");
      return null;
    }
    todos.push({ id: uuidv4(), text: todoText, complete: false });
    todoText = "";
    todos = todos;
  };

  const completeTodo = (event) => {
    let id = event.detail.id;
    let completedTodoIndex = todos.findIndex((todo) => todo.id === id);
    todos[completedTodoIndex] = {
      ...todos[completedTodoIndex],
      complete: true,
    };
    todos = todos;
  };

  const deleteTodo = (event) => {
    let id = event.detail.id;
    todos = todos.filter((todo) => todo.id !== id);
  };

  const onKeyPress = (event) => {
    if (event.charCode == 13) addTodo();
  };
</script>

<main>
  <div class="container">
    <div class="action">
      <input bind:value={todoText} on:keypress={onKeyPress} />
      <button on:click={addTodo}>Ekle</button>
    </div>
    <div class="todos">
      {#each todos as todo, index (todo.id)}
        <Todo
          on:delete={deleteTodo}
          on:complete={completeTodo}
          {todo}
          {index}
        />
      {/each}
    </div>
  </div>
</main>

<style>
  main {
    padding: 2rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .container {
    width: 100%;
    max-width: 560px;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .action {
    display: flex;
    gap: 8px;
    justify-content: space-between;
  }
  input {
    border: 0;
    flex: 3;
    width: 100%;
    background-color: #dedede;
  }
  input:focus {
    outline: 1px solid #ff3938;
  }
  button {
    font-weight: 600;
    cursor: pointer;
    border: 0;
    flex: 1;
    color: #ff3938;
    background-color: #dedede;
    width: 100%;
  }
  .todos {
    padding: 8px;
    height: 540px;
    overflow: auto;
    border: 1px solid #dedede;
    border-radius: 10px;
    background-color: #fff;
    border-radius: 4px;
  }

</style>
