<script>
    import { onMount } from 'svelte';
    import { createClient, SupabaseClient } from '@supabase/supabase-js';

    const supabaseUrl = 'https://your-supabase-url.supabase.co';
    const supabaseKey = 'your-supabase-key';
    const supabase: SupabaseClient = createClient(supabaseUrl, supabaseKey);

    let tasks = [];
    let users = [];

    onMount(async () => {
      const { data: taskData, error } = await supabase.from('tasks').select('*');
      if (error) throw error;
      tasks = taskData;

      const { data: userData, error: userError } = await supabase.from('users').select('*');
      if (userError) throw error;
      users = userData;
    });

    async function assignTask(taskId, userId) {
      const { error } = await supabase
        .from('task_assignments')
        .insert([{ task_id: taskId, user_id: userId }]);
      if (error) throw error;
    }
  </script>

  <h1>Task List</h1>
  <ul>
    {#each tasks as task}
      <li>
        {task.title} - Due: {task.due_date}
        <button on:click={() => assignTask(task.id, users[0].id)}>Assign to User 1</button>
      </li>
    {/each}
  </ul>
