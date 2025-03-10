<script>
    import { user } from '$app/stores';
  </script>

  {#if $user}
    <nav>
      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/login">Login</a></li>
      </ul>
    </nav>
    <slot />
  {:else}
    <nav>
      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/login">Login</a></li>
      </ul>
    </nav>
    <slot />
  {/if}
