<script context="module">
  const modules = import.meta.glob("./**.svelte");

  function capitalize(value) {
    return value.replace(/(?:^|\s|-)\S/g, (x) => x.toUpperCase());
  }

  let body = [];
  for (let path in modules) {
    let pathSanitized = path.replace(".svelte", "").replace("./", "/");

    let title1 = pathSanitized.substring(pathSanitized.lastIndexOf("/") + 1);

    let link1 = pathSanitized.includes("index")
      ? pathSanitized.replace("/index", "")
      : pathSanitized;
    let link2 = link1.replace("", "/").replace("//", "/");
    let title2 = link2 === "/" ? title1.replace("index", "home") : title1;

    let title3 = link2.split("/");
    let indexName = title3[title3.length - 1];

    let title4 =
      title2 === "index" ? title2.replace("index", indexName) : title2;

    // console.log(indexName);

    let title = title4;
    let link = link2;

    title = capitalize(title);

    body.push({
      title,
      link,
    });
  }
  console.log(body);
  export const load = async () => {
    const menu = await Promise.all(body);
    return {
      props: {
        menu,
      },
    };
  };
</script>

<script>
  export let menu;
</script>

<ul>
  {#each menu as item}
    <li>
      <span class="title"><a href={item.link}>{item.title}</a></span>
      <span class="link">{item.link}</span>
    </li>
  {/each}
</ul>

<!-- <slot /> -->
<style>
  ul {
    margin: 100px;
  }

  .title a {
    float: left;
    text-decoration: none;
  }
  .link {
    float: right;
  }
</style>
