<script>
  let labels = "hacktoberfest";
  let lans = "";
  let repos = "";
  let orgs = "";
  let baseUrl =
    "https://github.com/issues?q=is:issue is:open sort:updated-desc";
  let searchUrl = "";

  let isAssigned = true

  $: {
    const labelTags = labels
      .split(",")
      .filter((tag) => tag)
      .map((l) => `label:"${l.trim()}"`)
      .join(" ");
    const lanTags = lans
      .split(",")
      .filter((tag) => tag)
      .map((l) => `language:${l.trim()}`)
      .join(" ");
    const repoTags = repos
      .split(",")
      .filter((tag) => tag)
      .map((r) => `repo:${r.trim()}`)
      .join(" ");
    const orgTags = orgs
      .split(",")
      .filter((tag) => tag)
      .map((o) => `org:${o.trim()}`)
      .join(" ");

    let noAssignee = " "
    if(isAssigned)
      noAssignee = " no:assignee "

    searchUrl = `${baseUrl}${noAssignee}${labelTags && labelTags} ${lanTags && lanTags} ${
      repoTags && repoTags
    } ${orgTags && orgTags}`;
  }

  const onKeyPress = e => {
    if (e.charCode === 13) window.open(searchUrl, '_blank').focus();
  }
</script>

<main>
  <section class="container">
    <div class="search-form" on:keypress={onKeyPress}>
      <img
        class="form-logo"
        src="/hacktoberfest.svg"
        alt="Hacktoberfest 2021"
        width="200px"
      />
      <h1 class="form-heading">Search Issues in Github!</h1>
      <div class="form-input">
        <label for="label">Labels</label>
        <input
          bind:value={labels}
          name="label"
          type="text"
          placeholder="comma separated labels."
        />
      </div>
      <div class="form-input">
        <label for="language">Languages</label>
        <input
          bind:value={lans}
          name="language"
          type="text"
          placeholder="comma separated language."
        />
      </div>
      <div class="form-input">
        <label for="repo">Repositories</label>
        <input
          bind:value={repos}
          name="repo"
          type="text"
          placeholder="comma separated repos."
        />
      </div>
      <div class="form-input">
        <label for="org">Organizations</label>
        <input
          bind:value={orgs}
          name="org"
          type="text"
          placeholder="comma separated orgs."
        />
      </div>
      <div class="form-input">
        <label for="no-assignee">Not assigned to anyone</label>
        <input
          name="no-assignee"
          type="checkbox"
          bind:checked={isAssigned}
        />
      </div>
      <a class="form-button" href={searchUrl} target="_blank">
        View recent issues in Github
      </a>
    </div>
  </section>
</main>

<style>
  .container {
    width: 100vw;
    height: 100vh;
    background: rgba(43, 53, 49, 1);
    background: linear-gradient(
      29deg,
      rgba(180, 58, 37, 1) 50%,
      rgba(43, 53, 49, 1) 50%
    );
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .search-form {
    width: 600px;
    height: 600px;
    background-color: #f4f0e1;
    border-radius: 10px;
    background: #f4f0e1;
    box-shadow: 10px 10px 52px #525252;
    padding: 20px;
    color: #2b3531;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .form-heading {
    font-weight: bold;
    font-size: 1.7rem;
    margin-bottom: 20px;
  }

  .form-input {
    padding: 10px 0px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    max-width: 400px;
  }

  .form-input input {
    padding: 10px;
    border: 0.5px solid rgba(43, 53, 49, 1);
    border-radius: 7px;
    width: 200px;
  }

  .form-button {
    background-color: rgba(43, 53, 49, 1);
    color: white;
    padding: 8px 10px;
    border-radius: 4px;
    border: 0;
    margin: 20px 10px;
  }

  .form-logo {
    margin-bottom: 20px;
  }

  a {
    text-decoration: none;
    font-size: 17px;
  }
</style>
