<script>
  import { tasks } from '../store.js';

  let newTask = '';

  async function addTask() {
    const { error } = await supabase.from('tasks').insert([{ title: newTask, due_date: new Date().toISOString() }]);
    if (error) throw error;
    newTask = '';
  }
</script>

<h2>Tasks</h2>
<input type="text" bind:value={newTask} placeholder="New Task" />
<button on:click={addTask}>Add Task</button>
<ul>
  {#each $tasks as task}
    <li>{task.title} - Due: {task.due_date}</li>
  {/each}
</ul>
