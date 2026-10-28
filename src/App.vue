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
@import url('https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@500;600;700;800&family=Barlow:wght@400;500;600;700&display=swap');

* {
  box-sizing: border-box;
}

:root {
  --page: #111111;
  --surface: #f2f0e9;
  --surface-raised: #ffffff;
  --ink: #171717;
  --muted: #8f8c85;
  --line: rgba(255, 255, 255, 0.13);
  --accent: #ff5728;
}

html {
  min-width: 320px;
  background: var(--page);
}

body {
  margin: 0;
  color: #f5f3ed;
  background-color: var(--page);
  background-image:
    linear-gradient(115deg, transparent 0 57%, rgba(255, 87, 40, 0.08) 57% 57.4%, transparent 57.4%),
    repeating-linear-gradient(135deg, rgba(255, 255, 255, 0.018) 0 1px, transparent 1px 10px),
    linear-gradient(135deg, #1d1d1d 0%, #101010 58%, #1e1714 100%);
  background-size: 100% 100%, 100% 100%, 100% 100%;
  font-family: 'Barlow', 'Avenir Next', sans-serif;
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
  top: 0;
  right: 0;
  width: min(45vw, 560px);
  height: 100%;
  background: repeating-linear-gradient(135deg, transparent 0 23px, rgba(255, 87, 40, 0.07) 23px 24px, transparent 24px 48px);
  clip-path: polygon(76% 0, 100% 0, 100% 100%, 0 100%);
  content: '';
}

.topbar {
  width: 100%;
  height: 76px;
  padding: 0 7.5%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid var(--line);
  background: rgba(13, 13, 13, 0.9);
}

.wordmark {
  display: inline-flex;
  align-items: center;
  gap: 9px;
  color: #f5f3ed;
  text-decoration: none;
  font-size: 18px;
  font-weight: 700;
  letter-spacing: 0;
  text-transform: uppercase;
}

.wordmark-mark {
  width: 27px;
  height: 27px;
  display: grid;
  place-items: center;
  border: 0;
  border-radius: 2px;
  background: var(--accent);
  color: #171717;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 17px;
  font-weight: 800;
  clip-path: polygon(0 0, 100% 0, 100% 72%, 72% 100%, 0 100%);
}

.topbar-note {
  color: #aaa7a0;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 2px;
}

footer {
  padding: 0 24px 24px;
  color: #88847d;
  text-align: center;
  font-family: 'Barlow', sans-serif;
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
    right: -80px;
    width: 240px;
  }
}
</style>
