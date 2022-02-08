<script>
  export let todo;
  export let index;
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  function actionTodo(type) {
    dispatch(type, {
      id: todo.id,
    });
  }
</script>

<main>
  <div
    data-toggle="tooltip"
    title={todo.text}
    style={`background-color:${index % 2 == 0 ? "#fecdd2" : "#be123b"};color:${
      index % 2 == 0 ? "#be123b" : "#fff"
    }`}
  >
    <p style={`text-decoration:${todo.complete && "line-through"}`}>
      {index + 1}. {todo.text}
    </p>
    {#if todo.complete}
      <span on:click={() => actionTodo("delete")}>Listeden kaldır</span>
    {:else}
      <span on:click={() => actionTodo("complete")}>Tamamlandı</span>
    {/if}
  </div>
</main>

<style>
  div {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0px 4px;
    font-weight: 700;
    cursor: default;
  }
  p {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  span {
    margin-left: 8px;
    cursor: pointer;
    color: #000;
    font-size: 16px;
  }
</style>
