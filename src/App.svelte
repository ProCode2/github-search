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
    <div class="search-form">
      <div class="badge">
        <svg width="224" height="224" viewBox="0 0 224 224" fill="none" xmlns="http://www.w3.org/2000/svg" class="globe">
          <path fill-rule="evenodd" clip-rule="evenodd" d="M208.979 167.991C178.056 221.55 109.569 239.901 56.0093 208.979C2.44951 178.056 -15.9014 109.569 15.0213 56.0093C45.9441 2.44951 114.431 -15.9014 167.991 15.0213C221.55 45.9441 239.901 114.431 208.979 167.991ZM35.7052 184.154C29.9638 169.391 29.0031 150.536 32.8872 130.242C27.1675 124.879 22.1166 119.513 17.8303 114.256C13.7802 109.289 10.372 104.37 7.72571 99.5969C4.09083 130.28 14.2172 161.451 35.7052 184.154ZM9.90682 87.4222C12.1098 94.0693 16.6206 101.697 23.2545 109.833C26.5697 113.899 30.385 118.055 34.6529 122.24C36.1532 116.23 38.0655 110.127 40.3916 103.997C40.7446 104.505 41.144 104.984 41.5871 105.427C42.9061 106.746 44.54 107.677 46.3122 108.149C43.8773 114.783 41.9565 121.355 40.5414 127.772C44.3205 131.18 48.3839 134.589 52.706 137.966C52.5773 138.623 52.5099 139.302 52.5099 139.996L52.5099 146.628C47.7391 143.053 43.2338 139.422 39.0245 135.773C37.8976 142.897 37.4188 149.77 37.5754 156.255C38.0817 177.226 45.1093 193.053 57.0257 201.341L66.7935 184.423C67.2401 186.297 68.1979 188.028 69.5823 189.413C70.0861 189.916 70.6359 190.364 71.2218 190.75L63.1432 204.743C76.1431 210.356 92.9488 208.368 110.887 198.581C116.582 195.474 122.294 191.623 127.9 187.085C119.38 184.139 110.515 180.496 101.502 176.193L101.502 168.415C112.744 173.95 123.734 178.421 134.07 181.772C135.235 180.707 136.392 179.613 137.541 178.49L146.994 178.49C147.108 178.49 147.222 178.488 147.335 178.484C145.517 180.427 143.672 182.301 141.806 184.105C147.564 185.708 153.071 186.935 158.25 187.773C168.612 189.45 177.473 189.542 184.331 188.127C191.46 181.348 197.749 173.444 202.917 164.491C210.279 151.741 214.662 138.088 216.282 124.339C210.834 124.431 204.88 123.939 198.565 122.917C186.197 120.915 172.201 116.841 157.492 110.864L157.492 103.291C165.012 106.447 172.329 109.09 179.315 111.201C184.411 95.8738 186.745 80.9994 186.425 67.7456C185.932 47.317 179.25 31.769 167.889 23.3173L157.472 41.3604C157.314 38.8141 156.232 36.4037 154.418 34.5892C154.183 34.3546 153.938 34.1322 153.685 33.9225L161.884 19.7204C148.749 13.5443 131.528 15.3721 113.113 25.419C104.109 30.3316 95.0597 37.105 86.4577 45.5119L77.0058 45.5119C76.8917 45.5119 76.7778 45.5138 76.6641 45.5175C95.3136 25.5891 116.731 12.8676 136.34 9.84974C108.271 3.14963 78.794 8.44328 54.9537 23.8428C57.3791 28.3235 60.6378 33.1012 64.6879 38.0683C66.9857 40.8864 69.5239 43.748 72.2865 46.6325C71.2958 47.131 70.3814 47.7879 69.5825 48.5867C68.6777 49.4915 67.9552 50.5445 67.4384 51.6886C64.4707 48.6007 61.7396 45.5277 59.2637 42.4911C55.221 37.5331 51.8179 32.6229 49.1737 27.8584C38.0763 36.1362 28.444 46.7582 21.0825 59.5088C15.9137 68.4614 12.2134 77.8588 9.90682 87.4222ZM174.878 196.103C150.123 214.592 118.066 221.408 87.6611 214.15C103.317 211.741 120.126 203.146 135.759 189.635C143.263 191.907 150.436 193.598 157.132 194.682C163.458 195.706 169.422 196.198 174.878 196.103ZM216.87 117.325C211.777 117.465 206.01 117.032 199.683 116.008C195.356 115.308 190.799 114.336 186.058 113.1C195.131 85.6334 195.829 59.2182 188.295 39.8466C208.132 60.8047 218.286 88.9795 216.87 117.325Z" fill="url(#paint0_linear_1_287)"/>
          <defs>
            <linearGradient id="paint0_linear_1_287" x1="30.1538" y1="30.1538" x2="110.165" y2="227.733" gradientUnits="userSpaceOnUse">
              <stop stop-color="#FFE27D"/>
              <stop offset="0.505208" stop-color="#64E3FF"/>
              <stop offset="1" stop-color="#9192FF"/>
            </linearGradient>
          </defs>
        </svg>
        <svg width="113" height="124" viewBox="0 0 113 124" fill="none" xmlns="http://www.w3.org/2000/svg" class="h">
          <path d="M0.846191 49.8003L0.846202 38.2416L0.846964 37.5231H13.2913L13.2913 49.8001H2.10096L0.846191 49.8003Z" fill="currentColor"/>
          <path d="M13.2913 29.2604L13.2913 37.5231H25.7357L25.7357 86.6309H13.2913V91.5501L13.2906 98.908H24.5256L25.7357 98.9078V99.6507L25.735 106.835L25.7357 123.462H38.18V86.6308H50.6244V74.3542L75.5131 74.3539V111.185H87.9575V98.9078H97.7151L100.401 98.908L100.402 86.6309H105.388L112.463 86.6311L112.846 86.6309V74.3539L100.402 74.3539L100.402 86.6309L87.9575 86.6308L87.9575 0.692383L75.5131 0.692383V12.9693L63.0687 12.9693V25.2462H50.6244V28.9521L50.6237 36.4003L50.6244 37.5231H52.3251L63.0679 37.5234L63.0687 25.2462L75.3832 25.2462L75.5124 25.2464L75.5124 37.3024L75.5131 37.5231V44.1068L75.5124 51.0645L75.5131 62.077H50.6244V74.3542L38.18 74.3539V12.9693L25.7357 12.9693L25.735 17.3027L25.7357 25.2462H13.2913V29.2604Z" fill="currentColor"/>
        </svg>
      </div>
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
    font-family: monospace;
    width: 100vw;
    height: 100vh;
    background: #1d142a;
    background: linear-gradient(
      29deg,
      #1d142a,
      #353464
    );
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .search-form {
    width: 600px;
    height: 600px;
    background-color: #1d142a;
    border-radius: 10px;
    background: #353464;
    box-shadow: 10px 10px 52px #1d142a;
    padding: 20px;
    color: white;
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
    border: 0.5px solid #1d142a;
    border-radius: 7px;
    width: 200px;
  }

  .form-button {
    background-color: #1d142a;
    color: white;
    padding: 15px 10px;
    border-radius: 4px;
    border: 0;
    margin: 20px 10px;
  }
  .form-button:hover {
    background-color: rgba(43, 53, 49, 1);
  }
/*
  .form-logo {
    margin-bottom: 20px;
  } */

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
    color: gray;
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
  .badge {
    width: 100%;
    margin: 0px auto 24px;
    height: 224px;
    display: flex;
    -webkit-box-pack: center;
    -webkit-box-align: center;
    align-items: center;
    position: relative;
    text-align: center;
}
  .badge .globe {
    position: absolute;
    width: 100%;
}
  .badge .h {
    width: 50%;
    margin: 0px auto;
  color: white;
}
</style>
