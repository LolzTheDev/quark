<script lang="ts">
  let email = "";
  let username = "";
  let password = "";

  async function register() {
    const res = await fetch("/api/register", {
      method: "POST",
      body: JSON.stringify({ email, username, password }),
    });

    const data = await res.json();

    if (!data.valid) {
      alert(data.message);
      return;
    }

    await fetch("/api/login", {
      method: "POST",
      body: JSON.stringify({ username, password }),
    });

    window.location.href = "/feed";
  }
</script>

<main>
  <div
    class="bg-zinc-50 border-zinc-500 border rounded-lg p-6 flex justify-center w-1/5 mx-auto"
  >
    <div>
      <h1 class="text-3xl font-bold text-center">sign up</h1>
      <hr class="my-3 border-b-[1px] border-b-zinc-500" />
      <form on:submit={register} class="grid grid-cols-1 gap-2 w-64">
        <div class="space-y-1">
          <p>username</p>
          <input
            type="text"
            placeholder="mycoolusername"
            class="w-full"
            bind:value={username}
          />
        </div>

        <div class="space-y-1">
          <p>email</p>
          <input
            type="email"
            class="w-full"
            placeholder="me@example.com"
            bind:value={email}
          />
        </div>

        <div class="space-y-1">
          <p>password</p>
          <input
            type="password"
            class="w-full"
            placeholder="supersecretpassword123"
            bind:value={password}
          />
        </div>

        <input type="submit" class="mt-2" value="create account" />
      </form>
    </div>
  </div>
</main>
