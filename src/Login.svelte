<script>
  import { onMount } from 'svelte';
  import { createClient, SupabaseClient } from '@supabase/supabase-js';

  const supabaseUrl = 'https://your-supabase-url.supabase.co';
  const supabaseKey = 'your-supabase-key';
  const supabase: SupabaseClient = createClient(supabaseUrl, supabaseKey);

  let email = '';
  let password = '';

  async function login() {
    const { user, error } = await supabase.auth.signInWithPassword({ email, password });
    if (error) throw error;
    console.log('Logged in:', user);
  }
</script>

<h2>Login</h2>
<input type="email" bind:value={email} placeholder="Email" />
<input type="password" bind:value={password} placeholder="Password" />
<button on:click={login}>Login</button>
