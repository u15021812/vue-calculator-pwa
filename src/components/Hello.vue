<template>
  <section class="bmi-tool" aria-labelledby="page-title">
    <div class="intro">
      <p class="eyebrow"><span class="eyebrow-dot"></span> FERRAMENTA DE BEM-ESTAR</p>
      <h1 id="page-title">Seu corpo,<br><em>em números.</em></h1>
      <p class="intro-copy">O IMC é um ponto de partida para entender a relação entre seu peso e sua altura.</p>
      <div class="formula-note">
        <span class="formula-icon" aria-hidden="true">÷</span>
        <p><strong>Como calculamos</strong><br>peso (kg) ÷ altura (m)²</p>
      </div>
    </div>

    <div class="calculator">
      <div class="calculator-heading">
        <div>
          <p class="section-label">CALCULADORA</p>
          <h2>Vamos começar</h2>
        </div>
        <span class="step-mark" aria-hidden="true">01 <i>/ 01</i></span>
      </div>

      <form @submit.prevent="calculate">
        <label for="weight">Seu peso <span>quilogramas</span></label>
        <div class="input-wrap">
          <input
            id="weight"
            v-model.number="weight"
            type="number"
            min="1"
            max="500"
            step="0.1"
            inputmode="decimal"
            placeholder="Ex.: 68,5"
            required
          >
          <span class="unit">kg</span>
        </div>

        <label for="height">Sua altura <span>centímetros</span></label>
        <div class="input-wrap">
          <input
            id="height"
            v-model.number="height"
            type="number"
            min="50"
            max="250"
            step="1"
            inputmode="numeric"
            placeholder="Ex.: 170"
            required
          >
          <span class="unit">cm</span>
        </div>

        <button type="submit">Calcular meu IMC <span aria-hidden="true">→</span></button>
      </form>

      <section v-if="result !== null" class="result" aria-live="polite" aria-label="Resultado do IMC">
        <div class="result-topline">
          <p class="section-label">SEU RESULTADO</p>
          <span class="result-category" :class="resultClass">{{ category }}</span>
        </div>
        <div class="result-score">
          <strong>{{ formattedResult }}</strong>
          <span>kg/m²</span>
        </div>
        <div class="scale" aria-hidden="true">
          <span class="scale-marker" :style="{ left: markerPosition + '%' }"></span>
        </div>
        <div class="scale-labels"><span>Baixo</span><span>Adequado</span><span>Elevado</span></div>
        <p class="result-message">{{ resultMessage }}</p>
      </section>
      <p v-else class="privacy-note"><span aria-hidden="true">↗</span> Seus dados ficam apenas neste dispositivo.</p>
    </div>
  </section>
</template>

<script>
export default {
  name: 'BmiCalculator',
  data () {
    return {
      weight: null,
      height: null,
      result: null
    }
  },
  computed: {
    formattedResult () {
      return this.result === null ? '' : this.result.toFixed(1).replace('.', ',')
    },
    category () {
      if (this.result < 18.5) return 'Abaixo do peso'
      if (this.result < 25) return 'Faixa adequada'
      if (this.result < 30) return 'Sobrepeso'
      return 'Obesidade'
    },
    resultClass () {
      if (this.result < 18.5) return 'category-low'
      if (this.result < 25) return 'category-healthy'
      if (this.result < 30) return 'category-high'
      return 'category-very-high'
    },
    resultMessage () {
      if (this.result < 18.5) return 'O resultado está abaixo da faixa de referência para adultos.'
      if (this.result < 25) return 'O resultado está dentro da faixa de referência para adultos.'
      if (this.result < 30) return 'O resultado está acima da faixa de referência para adultos.'
      return 'O resultado está acima da faixa de referência para adultos.'
    },
    markerPosition () {
      return Math.max(3, Math.min(97, ((this.result - 15) / 25) * 100))
    }
  },
  methods: {
    calculate () {
      if (!this.weight || !this.height || this.weight <= 0 || this.height <= 0) {
        this.result = null
        return
      }

      const heightInMeters = this.height / 100
      this.result = this.weight / (heightInMeters * heightInMeters)
    }
  }
}
</script>

<style scoped>
.bmi-tool {
  position: relative;
  width: min(100%, 1050px);
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(350px, 440px);
  align-items: center;
  gap: clamp(52px, 8vw, 118px);
  animation: rise-in 550ms ease-out both;
}

.intro {
  position: relative;
  padding: 12px 0 20px 20px;
  border-left: 3px solid #ff5728;
}

.eyebrow,
.section-label {
  margin: 0;
  color: #b6b2aa;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 1.8px;
  text-transform: uppercase;
}

.eyebrow {
  display: flex;
  align-items: center;
  gap: 9px;
}

.eyebrow-dot {
  width: 8px;
  height: 8px;
  background: #ff5728;
  clip-path: polygon(0 0, 100% 0, 100% 70%, 70% 100%, 0 100%);
}

h1 {
  margin: 25px 0 20px;
  color: #f5f3ed;
  font-family: 'Barlow Condensed', Impact, sans-serif;
  font-size: clamp(58px, 7vw, 86px);
  font-weight: 800;
  line-height: 0.88;
  text-transform: uppercase;
}

h1 em {
  color: #ff5728;
  font-style: normal;
  font-weight: 800;
}

.intro-copy {
  max-width: 355px;
  margin: 0;
  color: #aaa7a0;
  font-size: 15px;
  line-height: 1.65;
}

.formula-note {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-top: 42px;
}

.formula-icon {
  width: 42px;
  height: 42px;
  display: grid;
  place-items: center;
  border: 1px solid rgba(255, 87, 40, 0.48);
  border-radius: 2px;
  background: rgba(255, 87, 40, 0.1);
  color: #ff7047;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 23px;
  font-weight: 700;
  clip-path: polygon(0 0, 100% 0, 100% 75%, 75% 100%, 0 100%);
}

.formula-note p {
  margin: 0;
  color: #aaa7a0;
  font-size: 11px;
  line-height: 1.7;
}

.formula-note strong {
  color: #f5f3ed;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 14px;
  font-weight: 700;
  text-transform: uppercase;
}

.calculator {
  position: relative;
  overflow: hidden;
  padding: 31px;
  border: 0;
  border-radius: 3px;
  background: var(--surface);
  color: #171717;
  box-shadow: 12px 12px 0 rgba(255, 87, 40, 0.18), 0 22px 65px rgba(0, 0, 0, 0.32);
}

.calculator .section-label {
  color: #77736c;
}

.calculator::before {
  position: absolute;
  top: 0;
  left: 31px;
  width: 72px;
  height: 4px;
  background: #ff5728;
  content: '';
}

.calculator-heading,
.result-topline {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.calculator-heading {
  margin-bottom: 27px;
  padding-bottom: 18px;
  border-bottom: 1px solid #d8d5cc;
}

.calculator-heading h2 {
  margin: 7px 0 0;
  color: #171717;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 30px;
  font-weight: 700;
  line-height: 1;
  text-transform: uppercase;
}

.step-mark {
  color: #ff5728;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 13px;
  font-weight: 700;
}

.step-mark i {
  color: #9f9b93;
  font-style: normal;
  font-weight: 400;
}

form label {
  display: flex;
  justify-content: space-between;
  margin: 0 0 8px;
  color: #242424;
  font-size: 13px;
  font-weight: 700;
}

form label span {
  color: #8b8881;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 11px;
  font-weight: 400;
  text-transform: uppercase;
}

.input-wrap {
  position: relative;
  margin-bottom: 19px;
}

.input-wrap input {
  width: 100%;
  height: 52px;
  padding: 0 55px 0 14px;
  border: 1px solid #d3d0c8;
  border-radius: 2px;
  outline: none;
  background: #fffefa;
  color: #171717;
  font-family: 'Barlow', sans-serif;
  font-size: 15px;
  font-weight: 600;
  transition: border-color 160ms ease, background 160ms ease, box-shadow 160ms ease;
}

.input-wrap input::placeholder {
  color: #aaa69d;
}

.input-wrap input:focus {
  border-color: #ff5728;
  background: #fff;
  box-shadow: 0 0 0 3px rgba(255, 87, 40, 0.14);
}

.unit {
  position: absolute;
  top: 50%;
  right: 15px;
  transform: translateY(-50%);
  color: #d7421c;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 12px;
  font-weight: 700;
  text-transform: uppercase;
}

button {
  width: 100%;
  height: 52px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 5px;
  padding: 0 16px;
  border: 0;
  border-radius: 2px;
  background: #ff5728;
  color: #171717;
  cursor: pointer;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 13px;
  font-weight: 700;
  text-transform: uppercase;
  transition: background 160ms ease, transform 160ms ease, box-shadow 160ms ease;
}

button:hover {
  transform: translateY(-2px);
  background: #ff754e;
  box-shadow: 0 8px 20px rgba(255, 87, 40, 0.22);
}

button:focus-visible {
  outline: 2px solid #171717;
  outline-offset: 3px;
}

button span {
  font-size: 20px;
  font-weight: 400;
}

.privacy-note {
  margin: 18px 0 0;
  color: #77736c;
  text-align: center;
  font-family: 'Barlow', sans-serif;
  font-size: 11px;
}

.privacy-note span {
  margin-right: 5px;
  color: #d7421c;
}

.result {
  margin-top: 25px;
  padding-top: 21px;
  border-top: 1px solid #d8d5cc;
  animation: reveal 300ms ease-out both;
}

.result-topline {
  gap: 12px;
}

.result-category {
  padding: 5px 8px;
  border: 1px solid transparent;
  border-radius: 2px;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
}

.category-low { border-color: #c6d9e8; background: #e2edf5; color: #41647f; }
.category-healthy { border-color: #c9df9e; background: #e7f0d7; color: #507126; }
.category-high { border-color: #f0c797; background: #f7e8d5; color: #895b25; }
.category-very-high { border-color: #e8b3a5; background: #f5e1db; color: #97412d; }

.result-score {
  display: flex;
  align-items: baseline;
  gap: 8px;
  margin: 11px 0 15px;
}

.result-score strong {
  color: #171717;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 48px;
  font-weight: 700;
  line-height: 1;
}

.result-score span {
  color: #77736c;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 11px;
  font-weight: 600;
}

.scale {
  position: relative;
  height: 5px;
  border-radius: 2px;
  background: linear-gradient(90deg, #6ea3cc 0%, #6ea3cc 14%, #9ac955 14%, #9ac955 40%, #e4a544 40%, #e4a544 60%, #e16e52 60%, #e16e52 100%);
}

.scale-marker {
  position: absolute;
  top: 50%;
  width: 12px;
  height: 12px;
  border: 2px solid #f2f0e9;
  border-radius: 50%;
  background: #171717;
  box-shadow: 0 0 0 1px #171717;
  transform: translate(-50%, -50%);
  transition: left 300ms ease;
}

.scale-labels {
  display: flex;
  justify-content: space-between;
  margin-top: 9px;
  color: #77736c;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 10px;
  font-weight: 600;
  text-transform: uppercase;
}

.result-message {
  margin: 15px 0 0;
  color: #5f5c55;
  font-size: 11px;
  line-height: 1.55;
}

@keyframes rise-in {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes reveal {
  from { opacity: 0; transform: translateY(5px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (max-width: 760px) {
  .bmi-tool {
    max-width: 520px;
    grid-template-columns: 1fr;
    gap: 28px;
  }

  .intro {
    padding: 0 0 0 16px;
  }

  h1 {
    margin: 18px 0 14px;
    font-size: 60px;
  }

  .intro-copy {
    max-width: 440px;
  }

  .formula-note {
    display: none;
  }
}

@media (max-width: 420px) {
  .calculator {
    padding: 26px 20px 22px;
  }

  .calculator::before {
    left: 20px;
  }

  h1 {
    font-size: 48px;
  }
}
</style>
