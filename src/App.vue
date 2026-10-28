<template>
  <div id="app">
    <header class="topbar">
      <a class="wordmark" href="#inicio" aria-label="Índice, início">
        <span class="wordmark-mark" aria-hidden="true">i</span>
        <span>índice</span>
      </a>
      <span class="topbar-note">SAÚDE EM PERSPECTIVA</span>
    </header>
    <main id="inicio">
      <hello></hello>
    </main>
    <footer>Uma referência para acompanhar sua saúde, não um diagnóstico.</footer>
  </div>
</template>

<script>
import Hello from './components/Hello'

export default {
  name: 'app',
  components: {
    Hello
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=Space+Grotesk:wght@400;500;600;700&display=swap');

* {
  box-sizing: border-box;
}

:root {
  --page: #08110f;
  --surface: #111b18;
  --surface-raised: #17231e;
  --ink: #e8f3ec;
  --muted: #8d9f95;
  --line: rgba(165, 195, 175, 0.16);
  --lime: #c3ff65;
  --cyan: #62e5d1;
}

html {
  min-width: 320px;
  background: var(--page);
}

body {
  margin: 0;
  color: var(--ink);
  background-color: var(--page);
  background-image:
    linear-gradient(rgba(141, 181, 155, 0.045) 1px, transparent 1px),
    linear-gradient(90deg, rgba(141, 181, 155, 0.045) 1px, transparent 1px),
    linear-gradient(125deg, #0d1915 0%, #08110f 52%, #0c1518 100%);
  background-size: 42px 42px, 42px 42px, 100% 100%;
  font-family: 'Space Grotesk', 'Avenir Next', sans-serif;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

main {
  position: relative;
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 48px 24px 64px;
  isolation: isolate;
}

main::before {
  position: absolute;
  z-index: -1;
  top: 10%;
  right: 7%;
  width: 280px;
  height: 280px;
  border: 1px solid rgba(98, 229, 209, 0.08);
  border-radius: 50%;
  content: '';
  box-shadow: 0 0 0 32px rgba(98, 229, 209, 0.018), 0 0 0 64px rgba(98, 229, 209, 0.012);
}

.topbar {
  width: 100%;
  height: 76px;
  padding: 0 7.5%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid var(--line);
  background: rgba(8, 17, 15, 0.72);
}

.wordmark {
  display: inline-flex;
  align-items: center;
  gap: 9px;
  color: var(--ink);
  text-decoration: none;
  font-size: 19px;
  font-weight: 600;
  letter-spacing: 0;
}

.wordmark-mark {
  width: 27px;
  height: 27px;
  display: grid;
  place-items: center;
  border: 1px solid var(--lime);
  border-radius: 5px 1px 5px 1px;
  background: rgba(195, 255, 101, 0.08);
  color: var(--lime);
  font-family: 'IBM Plex Mono', monospace;
  font-size: 16px;
  box-shadow: 0 0 16px rgba(195, 255, 101, 0.1);
}

.topbar-note {
  color: var(--muted);
  font-family: 'IBM Plex Mono', monospace;
  font-size: 9px;
  font-weight: 500;
  letter-spacing: 1.6px;
}

footer {
  padding: 0 24px 24px;
  color: #74867c;
  text-align: center;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
}

@media (max-width: 600px) {
  .topbar {
    height: 66px;
    padding: 0 22px;
  }

  main {
    align-items: flex-start;
    padding: 34px 18px 48px;
  }

  footer {
    padding: 0 20px 20px;
    line-height: 1.5;
  }

  main::before {
    top: 5%;
    right: -170px;
    width: 230px;
    height: 230px;
  }
}
</style>
