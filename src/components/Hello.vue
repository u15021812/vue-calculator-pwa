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
  animation: rise-in 650ms cubic-bezier(0.2, 0.8, 0.2, 1) both;
}

.intro {
  position: relative;
  padding: 12px 0 20px 20px;
  border-left: 1px solid rgba(195, 255, 101, 0.28);
}

.eyebrow,
.section-label {
  margin: 0;
  color: #91a69a;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 9px;
  font-weight: 500;
  letter-spacing: 1.4px;
}

.eyebrow {
  display: flex;
  align-items: center;
  gap: 9px;
}

.eyebrow-dot {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: #c3ff65;
  box-shadow: 0 0 12px rgba(195, 255, 101, 0.75);
}

h1 {
  margin: 25px 0 20px;
  color: #e8f3ec;
  font-family: 'Space Grotesk', sans-serif;
  font-size: clamp(50px, 6.2vw, 76px);
  font-weight: 600;
  line-height: 0.96;
}

h1 em {
  color: #c3ff65;
  font-style: normal;
  font-weight: 500;
  text-shadow: 0 0 26px rgba(195, 255, 101, 0.14);
}

.intro-copy {
  max-width: 355px;
  margin: 0;
  color: #9aaba1;
  font-size: 14px;
  line-height: 1.7;
}

.formula-note {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-top: 44px;
}

.formula-icon {
  width: 42px;
  height: 42px;
  display: grid;
  place-items: center;
  border: 1px solid rgba(98, 229, 209, 0.35);
  border-radius: 6px 1px 6px 1px;
  background: rgba(98, 229, 209, 0.06);
  color: #62e5d1;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 20px;
}

.formula-note p {
  margin: 0;
  color: #8d9f95;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
  line-height: 1.7;
}

.formula-note strong {
  color: #c4d2c9;
  font-family: 'Space Grotesk', sans-serif;
  font-size: 12px;
  font-weight: 500;
}

.calculator {
  position: relative;
  overflow: hidden;
  padding: 31px;
  border: 1px solid rgba(165, 195, 175, 0.2);
  border-radius: 5px;
  background: linear-gradient(145deg, rgba(23, 35, 30, 0.97), rgba(13, 23, 20, 0.98));
  box-shadow: 0 24px 80px rgba(0, 0, 0, 0.28), inset 0 1px 0 rgba(255, 255, 255, 0.035);
}

.calculator::before {
  position: absolute;
  top: 0;
  left: 31px;
  width: 58px;
  height: 2px;
  background: #c3ff65;
  box-shadow: 0 0 14px rgba(195, 255, 101, 0.6);
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
}

.calculator-heading h2 {
  margin: 7px 0 0;
  color: #edf5ef;
  font-family: 'Space Grotesk', sans-serif;
  font-size: 24px;
  font-weight: 500;
}

.step-mark {
  color: #c3ff65;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
  font-weight: 500;
}

.step-mark i {
  color: #65766c;
  font-style: normal;
  font-weight: 400;
}

form label {
  display: flex;
  justify-content: space-between;
  margin: 0 0 8px;
  color: #d5e0d8;
  font-size: 12px;
  font-weight: 500;
}

form label span {
  color: #74877c;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 9px;
  font-weight: 400;
}

.input-wrap {
  position: relative;
  margin-bottom: 19px;
}

.input-wrap input {
  width: 100%;
  height: 50px;
  padding: 0 55px 0 14px;
  border: 1px solid rgba(165, 195, 175, 0.2);
  border-radius: 3px;
  outline: none;
  background: rgba(5, 13, 11, 0.55);
  color: #e9f4ed;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 13px;
  transition: border-color 160ms ease, background 160ms ease, box-shadow 160ms ease;
}

.input-wrap input::placeholder {
  color: #5e7065;
}

.input-wrap input:focus {
  border-color: #62e5d1;
  background: rgba(8, 19, 16, 0.9);
  box-shadow: 0 0 0 3px rgba(98, 229, 209, 0.1), 0 0 20px rgba(98, 229, 209, 0.06);
}

.unit {
  position: absolute;
  top: 50%;
  right: 15px;
  transform: translateY(-50%);
  color: #62e5d1;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 10px;
}

button {
  width: 100%;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 5px;
  padding: 0 16px;
  border: 0;
  border-radius: 3px;
  background: #c3ff65;
  color: #152017;
  cursor: pointer;
  font-family: 'Space Grotesk', sans-serif;
  font-size: 13px;
  font-weight: 700;
  transition: background 160ms ease, transform 160ms ease, box-shadow 160ms ease;
}

button:hover {
  transform: translateY(-2px);
  background: #d2ff8a;
  box-shadow: 0 8px 26px rgba(195, 255, 101, 0.16);
}

button:focus-visible {
  outline: 2px solid #62e5d1;
  outline-offset: 3px;
}

button span {
  font-size: 20px;
  font-weight: 400;
}

.privacy-note {
  margin: 18px 0 0;
  color: #75867c;
  text-align: center;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 9px;
}

.privacy-note span {
  margin-right: 5px;
  color: #62e5d1;
}

.result {
  margin-top: 25px;
  padding-top: 21px;
  border-top: 1px solid rgba(165, 195, 175, 0.16);
  animation: reveal 300ms ease-out both;
}

.result-topline {
  gap: 12px;
}

.result-category {
  padding: 5px 8px;
  border: 1px solid transparent;
  border-radius: 2px;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 9px;
  font-weight: 600;
}

.category-low { border-color: rgba(122, 176, 224, 0.25); background: rgba(79, 133, 181, 0.12); color: #9ac9ef; }
.category-healthy { border-color: rgba(195, 255, 101, 0.24); background: rgba(195, 255, 101, 0.09); color: #c3ff65; }
.category-high { border-color: rgba(255, 194, 99, 0.24); background: rgba(255, 194, 99, 0.09); color: #ffc263; }
.category-very-high { border-color: rgba(255, 133, 112, 0.24); background: rgba(255, 133, 112, 0.09); color: #ff9a87; }

.result-score {
  display: flex;
  align-items: baseline;
  gap: 8px;
  margin: 11px 0 15px;
}

.result-score strong {
  color: #eef6ef;
  font-family: 'Space Grotesk', sans-serif;
  font-size: 44px;
  font-weight: 500;
  line-height: 1;
}

.result-score span {
  color: #7f9186;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 9px;
}

.scale {
  position: relative;
  height: 5px;
  border-radius: 2px;
  background: linear-gradient(90deg, #78acd4 0%, #78acd4 14%, #a8e55d 14%, #a8e55d 40%, #e8b75f 40%, #e8b75f 60%, #ed8f78 60%, #ed8f78 100%);
}

.scale-marker {
  position: absolute;
  top: 50%;
  width: 12px;
  height: 12px;
  border: 2px solid #111b18;
  border-radius: 50%;
  background: #e8f3ec;
  box-shadow: 0 0 0 1px #e8f3ec, 0 0 10px rgba(232, 243, 236, 0.45);
  transform: translate(-50%, -50%);
  transition: left 300ms ease;
}

.scale-labels {
  display: flex;
  justify-content: space-between;
  margin-top: 9px;
  color: #718278;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 8px;
}

.result-message {
  margin: 15px 0 0;
  color: #9aaba1;
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
    font-size: 56px;
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
