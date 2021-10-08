<script>
  import langsJSON from "./languages.json";

  let labels = "hacktoberfest";
  let lans = "";

  let repos = "";
  let orgs = "";
  let baseUrl =
    "https://github.com/issues?q=is:issue is:open sort:created-desc";
  let searchUrl = "";

  let isAssigned = true;

  let showLangsDropdown = false;
  let langsList = langsJSON;
  let part = "";

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

    let noAssignee = " ";
    if (isAssigned) noAssignee = " no:assignee ";

    searchUrl = `${baseUrl}${noAssignee}${labelTags && labelTags} ${
      lanTags && lanTags
    } ${repoTags && repoTags} ${orgTags && orgTags}`;
  }

  const onKeyPress = (e) => {
    if (e.charCode === 13) window.open(searchUrl, "_blank").focus();
  };

  // remove or add language
  const handleLang = (lang) => {
    let index = langsList.findIndex((x) => x.name == lang);
    if (!langsList[index].selected) {
      lans += lans.length > 0 ? `,${lang}` : `${lang}`;
      langsList[index].selected = true;
    } else {
      langsList[index].selected = false;

      // remove item from lans string
      let lansArray = lans.split(",");
      let itemToRemoveIndex = lansArray.indexOf(lang);
      lansArray.splice(itemToRemoveIndex, 1);
      lans = lansArray.join(",");
    }
    part = "";
    langsList = langsJSON;
  };

  const searchLang = () => {
    langsList = langsJSON.filter(
      (item) => item.name.toLowerCase().substring(0, part.length) === part
    );
  };

  const appendAllLangs = () => {
    lans = "";
    langsList.forEach((lang) => {
      lang.selected = true;
      lans += lans.length > 0 ? `,${lang.name}` : `${lang.name}`;
    });
    langsList[0].selected = true;
  };

  const removeAllLangs = () => {
    lans = "";
    langsList.forEach((lang) => {
      lang.selected = false;
    });
    langsList[0].selected = false;
  };
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
        <div>
          <input
            bind:value={lans}
            on:click={() => {
              showLangsDropdown = true;
            }}
            name="language"
            type="text"
            placeholder="comma separated language."
            autoComplete="off"
          />

          {#if showLangsDropdown}
            <div class="form-dropdown">
              <input
                bind:value={part}
                on:input={searchLang}
                type="text"
                placeholder="Search languages"
                autoComplete="off"
              />
              <div class="buttons-group">
                <button
                  on:click={() => {
                    appendAllLangs();
                  }}>select all</button
                >
                <button
                  on:click={() => {
                    removeAllLangs();
                  }}>remove all</button
                >
              </div>
              <div class="dropdown-items">
                {#each langsList as lang}
                  <div
                    on:click={() => handleLang(lang.name)}
                    class="dropdown-item"
                  >
                    <span>{lang.name}</span>
                    {#if lang.selected}
                      <img src="./checked.png" alt="" />
                    {/if}
                  </div>
                {/each}
              </div>
            </div>
          {/if}
        </div>
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
        <input name="no-assignee" type="checkbox" bind:checked={isAssigned} />
      </div>
      <a class="form-button" href={searchUrl} target="_blank">
        View recent issues in Github
      </a>
    </div>
  </section>

  {#if showLangsDropdown}
    <div
      id="dimmer"
      on:click={() => {
        showLangsDropdown = false;
      }}
    />
  {/if}
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
    border-radius: 4px;
    width: 200px;
  }

  .form-button {
    background-color: rgba(43, 53, 49, 0.9);
    color: white;
    padding: 15px 10px;
    border-radius: 4px;
    border: 0;
    margin: 20px 10px;
  }
  .form-button:hover {
    background-color: rgba(43, 53, 49, 1);
  }

  .form-logo {
    margin-bottom: 20px;
  }

  a {
    text-decoration: none;
    font-size: 17px;
  }

  input {
    font-size: 15px;
  }

  input:focus {
    box-shadow: 0 0 0 0.125em #f747004d;
    border: 1px solid #f74700 !important;
    outline: none;
  }

  .form-dropdown {
    position: absolute;
    background: #fff;
    margin-top: 2px;
    border: 1px solid lightgray;
    padding: 5px;
    width: 220px;
    z-index: 101;
  }

  .form-dropdown input {
    margin-bottom: 5px;
    padding: 10px;
    border: 0.5px solid rgba(43, 53, 49, 1);
    border-radius: 4px;
    width: initial !important;
  }

  .buttons-group {
    display: flex;
  }

  .buttons-group button {
    flex: 1;
    padding: 10px;
    cursor: pointer;
    border: 0;
    font-size: 15px;
    background-color: rgb(235, 235, 235);
  }

  .buttons-group button:hover {
    background-color: rgb(218, 218, 218);
    transition-delay: 0.2s;
  }

  .dropdown-items {
    max-height: 333px;
    overflow-y: auto;
    margin-top: 5px;
  }

  .dropdown-item {
    display: flex;
    justify-content: space-between;
    padding: 10px 5px;
    border-bottom: 1px solid lightgray;
    cursor: pointer;
  }

  .dropdown-item img {
    height: 15px;
  }

  #dimmer {
    width: 100%;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;
  }
</style>
