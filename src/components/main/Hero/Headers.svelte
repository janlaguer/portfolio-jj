<script>
  import { onMount } from "svelte";

  const sections = document.querySelectorAll(
    "#about, #skills, #experiences, #projects"
  );

  let aboutLink, skillslink, experiencesLink, projectsLink;
  let hasMounted = false;
  const linkVisibilityMap = {};

  onMount(() => {
    [aboutLink, skillslink, experiencesLink, projectsLink].map((link) => {
      linkVisibilityMap[link.id] = {
        isIntersecting: false,
        show: false,
        ref: link,
      };
    });
    const observer = new IntersectionObserver((entries) => {
      entries.map((entry) => {
        linkVisibilityMap[`${entry.target.id}link`]["isIntersecting"] =
          entry.isIntersecting;
      });

      let hasFoundCurrentSection = false;
      for (const linkID in linkVisibilityMap) {
        const linkObj = linkVisibilityMap[linkID];
        if (hasFoundCurrentSection || !linkObj.isIntersecting) {
          // remove the current class
          linkObj.show = false;
          continue;
        }

        // add the current class
        linkObj.show = true;
        hasFoundCurrentSection = true;
      }
    });

    sections.forEach((section) => observer.observe(section));
    hasMounted = true;

    return () => sections.forEach((section) => observer.unobserve(section));
  });
</script>

<div class="hidden xl:flex xl:flex-col gap-4 items-start tracking-tight">
  <a
    bind:this={aboutLink}
    class:current={hasMounted ? linkVisibilityMap[aboutLink.id].show : false}
    id="aboutlink"
    href="/#about">- ABOUT</a
  >
  <a
    bind:this={skillslink}
    class:current={hasMounted ? linkVisibilityMap[skillslink.id].show : false}
    id="skillslink"
    href="/#skills">- SKILLS</a
  >
  <a
    bind:this={experiencesLink}
    class:current={hasMounted
      ? linkVisibilityMap[experiencesLink.id].show
      : false}
    id="experienceslink"
    href="/#experiences">- EXPERIENCES</a
  >
  <a
    bind:this={projectsLink}
    class:current={hasMounted ? linkVisibilityMap[projectsLink.id].show : false}
    id="projectslink"
    href="/#projects">- PROJECTS</a
  >
</div>

<style>
  a {
    @apply text-gray-600 transition-all;
  }

  .current {
    @apply text-white font-medium scale-110 origin-left tracking-wide;
  }
</style>