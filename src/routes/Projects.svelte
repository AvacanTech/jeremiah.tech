<script>
    import { onMount } from "svelte";
    import axios  from "axios";

    const token = import.meta.env.GITHUB_TOKEN;
    /** @type {{ html_url: string; name: string; stargazers_count: number; }[]} */
    let projects = [];


    onMount(async () => {
  const response = await axios.get('https://api.github.com/search/repositories?q=created:>=2022-01-01&sort=stars&order=desc', {
    headers: {
      Authorization: token,
    },
  });
  projects = response.data.items;
});



</script>

<main>
    <h1>Projects</h1>
    {#if projects.length > 0}
      <ul>
        {#each projects as project}
          <li>
            <a href={project.html_url} target="_blank">{project.name}</a> - Stars: {project.stargazers_count}
          </li>
        {/each}
      </ul>
    {:else}
      <p>No repositories found.</p>
    {/if}
  </main>