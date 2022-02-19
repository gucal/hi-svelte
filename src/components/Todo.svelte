<script>
  export let todo;
  export let index;
  import { createEventDispatcher } from "svelte";
  import Icon from "@iconify/svelte";

  const dispatch = createEventDispatcher();

  function actionTodo(type) {
    dispatch(type, {
      id: todo.id,
    });
  }
</script>

<main>
  <div data-toggle="tooltip" title={todo.text}>
    <p style={`text-decoration:${todo.complete && "line-through"}`}>
      {index + 1}. {todo.text}
    </p>
    {#if todo.complete}
      <span title="Listeden Kaldır" on:click={() => actionTodo("delete")}>
        <Icon
          icon="mdi:close-circle"
          style={"color:red;font-size:22px"}
        /></span
      >
    {:else}
      <span title="Tamamlandı" on:click={() => actionTodo("complete")}
        ><Icon
          icon="mdi:check-circle"
          style={"color:green;font-size:22px"}
        /></span
      >
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
