<script lang="ts" context="module">
  import Todos from './Components/Todos.svelte';

  // An interface is declared to define the shape of the object we expect to receive from the mock API. See https://www.typescriptlang.org/docs/handbook/interfaces.html for more info on interfaces.

  export interface TodoObject {
    id: number;
    body: string;
    completed: boolean;
  }
</script>

<main>
  <h1>TODOS</h1>
  <Todos />
</main>

<style>
</style>
