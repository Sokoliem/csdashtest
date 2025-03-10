<script>
    import { writable } from 'svelte/store';
    import { onMount } from 'svelte';
    import { createClient, SupabaseClient } from '@supabase/supabase-js';

    const supabaseUrl = 'https://your-supabase-url.supabase.co';
    const supabaseKey = 'your-supabase-key';
    const supabase: SupabaseClient = createClient(supabaseUrl, supabaseKey);

    let user = writable(null);
    let error = writable(null);

    async function login(email, password) {
      const { data, error } = await supabase.auth.signInWithPassword({ email, password });
      if (error) {
        error.set(error.message);
      } else {
        user.set(data.user);
      }
    }

    onMount(() => {
      const authListener = supabase.auth.onAuthStateChange((event, session) => {
        if (session?.user) {
          user.set(session.user);
        } else {
          user.set(null);
        }
      });

      return () => {
        authListener.data.unsubscribe();
      };
    });
  </script>

  <h1>Login</h1>
  {#if $user}
    <p>Welcome, {$user.email}!</p>
  {:else}
    <form on:submit|preventDefault={async (event) => {
      const email = event.target.elements.email.value;
      const password = event.target.elements.password.value;
      await login(email, password);
    }}>
      <input type="email" name="email" placeholder="Email" required />
      <input type="password" name="password" placeholder="Password" required />
      <button type="submit">Login</button>
    </form>
  {/if}
