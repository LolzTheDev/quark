<script lang="ts">
  import Post from "$lib/components/Post.svelte";
  import type { PageServerData } from "./$types";
  import { page } from "$app/stores";

  export let data: PageServerData;
  let content = "";
  let status = "ok";
  let message = "";

  async function post() {
    if (!content) {
      status = "not ok";
      message = "post cannot be empty";
      return;
    }

    const res = await fetch("/api/post", {
      method: "POST",
      headers: {
        Authorization: $page.data.session,
      },
      body: JSON.stringify({ content }),
    });

    const json = await res.json();

    if (!json.ok) {
      status = "not ok";
      message = json.message;
    }

    // set href to prevent state preserving
    window.location.href = "/feed";
  }
</script>

<svelte:head>
  <title>quark - my feed</title>

  <meta name="og:title" content="feed" />

  <meta name="og:description" content="view your personalized posts" />

  <meta name="theme-color" content="#0ea5e9" />
</svelte:head>

<main>
  <h1 class="text-center text-4xl font-bold">your feed</h1>

  {#if status === "not ok"}
    <div
      class="mt-4 bg-red-100 rounded-md border-red-400 border-2 mx-auto w-96 p-2"
    >
      <h1 class="text-lg font-bold">error</h1>
      <h1 class="font-medium">
        {message}
      </h1>
    </div>
  {/if}

  <div class="lg:grid p-2 lg:grid-cols-[440px_minmax(500px,_1fr)_440px] gap-4">
    <div
      class="lg:opacity-100 sticky top-28 opacity-0 bg-zinc-50 border rounded-xl h-[8.2%] p-5 mr-6 border-zinc-300"
    >
      <h2 class="text-3xl font-bold">quark open beta</h2>

      <hr class="border-b border-b-zinc-300 my-4" />

      <p>the official quark instance!</p>
      <p>
        this repository can be viewed at <a
          class="text-sky-600"
          href="https://github.com/lolzthedev/quark">LolzTheDev/quark</a
        >
      </p>

      <hr class="border-b border-b-zinc-300 my-4" />

      <h2 class="text-lg font-bold">rules</h2>
      <ol class="list-decimal ml-5">
        <li>no (heavily) nsfw posts</li>
        <li>no botting statistics</li>
        <li>no spamming, use of slurs, or offensive jokes</li>
      </ol>

      <hr class="border-b border-b-zinc-300 my-4" />

      <h2 class="text-lg font-bold">your data</h2>
      <ol class="list-decimal ml-5">
        <li>your ip is temporarily logged but not stored</li>
        <li>your email is only used for password recovery (soon&TRADE;)</li>
      </ol>
    </div>

    <div>
      <div class="flex w-full justify-center">
        <form onsubmit={post} class="my-4">
          <input
            type="text"
            placeholder="post anything..."
            bind:value={content}
          />
          <input type="submit" value="create" />
        </form>
      </div>

      {#if data.posts.length === 0}
        <p class="text-center">looking empty...</p>
      {:else}
        <div class="flex w-full justify-center">
          <div class="space-y-3">
            {#each data.posts as post}
              <Post {post} />
            {/each}
          </div>
        </div>
      {/if}
    </div>
  </div>
</main>
