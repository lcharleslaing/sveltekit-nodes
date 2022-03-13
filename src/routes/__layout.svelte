<script context="module">
  const modules = import.meta.glob("./**.svelte");

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
      <a href={item.link}>{item.title}</a>
    </li>
  {/each}
</ul>

<slot />
