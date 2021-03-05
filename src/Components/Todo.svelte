<script lang="ts">
  import axios from 'axios';
  import type { TodoObject } from '../App.svelte';

  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  // Prop declarations. See https://svelte.dev/tutorial/declaring-props
  export let body: string;
  export let completed: boolean;
  export let id: number;

  // A put request is submitted to update the completed status of the todo. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT

  function handleComplete(value) {
    const todoObject: TodoObject = {
      id: id,
      body: body,
      completed: value,
    };
    axios
      .put(`http://localhost:3000/todos/${id}`, todoObject)
      .then(() => {
        completed = value;
      })
      .catch((err) => {
        console.log(err);
      });
  }

  // handleDelete uses the Svelte event dispatcher to send an event up to the parent component (Todos.svelte). We do this so we can update the todos array handling our client side state. See: https://svelte.dev/tutorial/component-events

  function handleDelete() {
    axios.delete(`http://localhost:3000/todos/${id}`).then(() => {
      dispatch('delete', {
        id: id,
      });
    });
  }
</script>

<li>
  <!-- The class directive on is bound to the .completed CSS class. It is then made conditional based on if the completed prop value is true or not. See: https://svelte.dev/tutorial/classes -->

  <!-- The body value will take in the value passed in through the body prop defined in Todos.svelte -->

  <div class:completed={completed === true}>{body}</div>

  <!-- https://svelte.dev/tutorial/if-blocks -->

  <!-- The if block allows for conditional rendering of elements on the component. "If completed is true, render the following two buttons. Otherwise, render one button" -->

  <!-- With the on:click directive, we call a function from a function to allow a parameter to be passed to the handleComplete function. If the wrapping arrow function is removed, handleComplete attempts to call immediately rather than waiting for the button click. Note: This is not exclusive the the click event. This will happen with all events -->

  {#if completed}
    <button on:click={() => handleComplete(false)}>Undo</button>
    <button on:click={handleDelete}>Delete</button>
  {:else}
    <button on:click={() => handleComplete(true)}>Complete</button>
  {/if}
</li>

<style>
  /* Styles in Svelte are scoped to their component */

  .completed {
    text-decoration: line-through;
  }
</style>
