<!-- // src/routes/auth/+page.svelte -->
<script>
    export let data
    let { supabase } = data
    $: ({ supabase } = data)
  
    let email
    let password
    $: user = false
    const handleSignUp = async () => {
      await supabase.auth.signUp({
        email,
        password,
        options: {
          emailRedirectTo: `${location.origin}/auth/callback`,
        },
      })
    }
  
    const handleSignIn = async () => {
      await supabase.auth.signInWithPassword({
        email,
        password,
      })
    }
  
    const handleSignOut = async () => {
      await supabase.auth.signOut()
    }
   $: supabase.auth.getUser().then((current_user) => {
     
      if(current_user.data.user) {
        user = true
      }else{
        user = false
      }

    })
  </script>
  
  <form on:submit="{handleSignUp}">
    <input name="email" bind:value="{email}" />
    <input type="password" name="password" bind:value="{password}" />
    <button>Sign up</button>
  </form>
  
  {#if user}
  <button on:click="{handleSignOut}">Sign out</button>
<div class="d">
  djads
</div>

  {:else}
  <button on:click="{handleSignIn}">Sign in</button>

  {/if}
