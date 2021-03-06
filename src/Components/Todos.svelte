<script lang="ts">
  // Axios is the http request library we'll be using. It's more simple than the Fetch API
  import axios from 'axios';
  import { onMount } from 'svelte';

  import Todo from './Todo.svelte';

  // We import the type declared by our interface in App.svelte

  import type { TodoObject } from '../App.svelte';

  // todos is defined as an array of Todos per our mock API

  let todos: TodoObject[] = [];

  // The $: label declares a value as reactive. You can think of this like a calculated field. To test how this works, try replacing '$:' with 'let'. See: https://svelte.dev/tutorial/reactive-declarations

  $: totalTasks = todos.length;

  // onMount is a lifecycle method that is called once the component is first rendered. This is typically where you'll find initial data requests. For a more in-depth explanation, see: https://svelte.dev/tutorial/onmount

  // onMount is calling an ES6 arrow function. See https://javascript.info/arrow-functions-basics

  // getRequest is using promise chaining syntax. A promise is created from the get request and once the promise has been resolved, the function declared in then() is executed. If there is an error resolving the promise, then() is skipped and the function in catch() is executed. See https://javascript.info/promise-chaining for more info.

  onMount(() => {
    const getRequest: Promise<void> = axios
      .get('http://localhost:3000/todos')
      .then((res) => {
        todos = res.data;
      })
      .catch((err) => {
        console.log(err);
      });
  });

  // Another alternative syntax to promise chaining is the async/await syntax. The above function could be declared in the following way using async/await. See https://javascript.info/async-await for more details.

  //   onMount(() => {
  //     const getRequest: () => Promise<void> = async () => {
  //       try {
  //         const res = await axios.get('http://localhost:3000/todos');
  //         todos = res.data;
  //       } catch (err) {
  //         console.log(err);
  //       }
  //     };
  //     getRequest();
  //   });

  // Defined the variable to take the input
  let task: string;

  // handleSubmit will be called when the Submit button is pressed. We defined a todoObject that will take an id, body (value of the task variable), and pass a default completed value of false.

  // The todoObject is sent as as post request to our mock API. If successful, the local state of our application is updated to add the new todo. See: https://svelte.dev/tutorial/updating-arrays-and-objects

  function handleSubmit() {
    const todoObject: TodoObject = {
      id: todos.length + 1,
      body: task,
      completed: false,
    };
    axios
      .post('http://localhost:3000/todos', todoObject)
      .then(() => {
        todos = [...todos, todoObject];
      })
      .catch((err) => {
        console.log(err);
      });
  }

  // An event is emitted from the Delete button in Todo.svelte when pressed. The event is then caught by the function and the todos state is updated to reflect the deleted todo.

  function catchDelete(event) {
    todos = todos.filter((todo) => todo.id != event.detail.id);
  }
</script>

<!-- The "on" directive allows us to run a function when a given event is called. In this case, when the form is submitted, the handleSubmit() function is called -->

<!-- preventDefault is set on the on:submit directive to prevent the default form action reloading the page. See https://svelte.dev/tutorial/event-modifiers -->
<h3>Total Tasks: {totalTasks}</h3>

<form on:submit|preventDefault={handleSubmit}>
  <!-- In our input, we bind the value of the input to the task variable. Whenever we make a change in the input, the task variable is updated. -->

  <input type="text" bind:value={task} placeholder="Add a task..." />
  <button type="submit">Add Task</button>
</form>

<ul>
  <!-- This is an example of a keyed foreach block. This can be interpreted as "For each todo in todos, create a Todo component with a key of id." See: https://svelte.dev/tutorial/keyed-each-blocks-->

  <!-- The props we exported in Todo.svelte are use in the <Todo/> component. Within the foreach block, the props are defined based on the values of the Todo object. -->

  <!-- The on:delete directive is set so the event from the Delete button can be handled in Todos.svelte -->

  {#each todos as todo (todo.id)}
    <Todo
      body={todo.body}
      completed={todo.completed}
      id={todo.id}
      on:delete={catchDelete}
    />
  {/each}
</ul>
