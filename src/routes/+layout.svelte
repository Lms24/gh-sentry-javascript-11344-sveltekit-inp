<script>
  import Header from "./Header.svelte";
  import "./styles.css";

  let worstInp = 0;

  const observer = new PerformanceObserver((list, obs, options) => {
    for (let entry of list.getEntries()) {
      if (!entry.interactionId) continue;

      entry.renderTime = entry.startTime + entry.duration;
      worstInp = Math.max(entry.duration, worstInp);

      console.log(
        "[Interaction]",
        entry.duration,
        `type: ${entry.name} interactionCount: ${performance.interactionCount}, worstInp: ${worstInp}`,
        entry,
        options
      );
    }
  });

  observer.observe({
    type: "event",
    durationThreshold: 0, // 16 minimum by spec
    buffered: true,
  });
</script>

<div class="app">
  <Header />

  <main>
    <slot />
  </main>

  <footer>
    <p>
      visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to learn SvelteKit
    </p>
  </footer>
</div>

<style>
  .app {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }

  main {
    flex: 1;
    display: flex;
    flex-direction: column;
    padding: 1rem;
    width: 100%;
    max-width: 64rem;
    margin: 0 auto;
    box-sizing: border-box;
  }

  footer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 12px;
  }

  footer a {
    font-weight: bold;
  }

  @media (min-width: 480px) {
    footer {
      padding: 12px 0;
    }
  }
</style>
