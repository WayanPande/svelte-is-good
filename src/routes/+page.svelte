<script>
  import { flip } from "svelte/animate";
  import { fade, fly } from "svelte/transition";

  let clicked = 0;
  let title = "";
  let todos = [
    { checked: false, title: "Eat" },
    { checked: false, title: "Study" },
    { checked: false, title: "Work Out" },
  ];
  let cheked = false;

  $: console.log(cheked);

  const add = () => clicked++;

  const addTodos = () => {
    if (title.length > 0) {
      todos = [...todos, { checked: false, title }];
      title = "";
    }
  };

  const removeTodo = (data) => {
    todos = todos.filter((todo) => todo !== data);
  };
</script>

<section class="flex flex-col gap-3 w-1/2">
  <h1 class="text-center text-2xl">My Todos</h1>

  <form on:submit={addTodos}>
    <label for="search" class="mb-2 text-sm font-medium text-gray-900 sr-only"
      >Add todo</label
    >
    <div class="relative">
      <div
        class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none"
      >
        <svg
          class="w-4 h-4 text-gray-500"
          aria-hidden="true"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 20 20"
        >
          <path
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"
          />
        </svg>
      </div>
      <input
        type="text"
        id="text"
        class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500"
        placeholder="working out..."
        required
        bind:value={title}
      />
      <button
        type="submit"
        class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2"
        >Search</button
      >
    </div>
  </form>

  <div class="mt-5 flex flex-col gap-3">
    {#each todos as todo, i (todo + i)}
      <div
        class="ring p-3 flex gap-3 rounded items-center"
        in:fly={{ y: 200, duration: 1000 }}
        out:fade
        animate:flip={{ duration: 200 }}
      >
        <div class="flex items-center">
          <input
            name={`checked-${i}`}
            id={`checked-${i}`}
            type="checkbox"
            bind:checked={todo.checked}
            class={`w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded focus:ring-blue-500 focus:ring-2`}
          />
          <label
            for={`checked-${i}`}
            class={`${
              todo.checked && "line-through"
            } ml-2 text-sm font-medium text-gray-900`}>{todo.title}</label
          >
        </div>

        <button
          on:click={() => removeTodo(todo)}
          type="button"
          class="ml-auto mr-0 text-red-700 hover:text-white border border-red-700 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:border-red-500 dark:text-red-500 dark:hover:text-white dark:hover:bg-red-600 dark:focus:ring-red-900"
          >Delete</button
        >
      </div>
    {/each}
  </div>
</section>
