<template>
  <div id='calculator'>
    <div class='row'>
      <div class='col-sm-6 mb-3'>
        <b-form @submit.stop.prevent='calculate' novalidate>
          <select id='spinner' v-model='calcType'>
            <option
              v-for='(option, i) in options'
              v-bind:key='i'
              v-bind:value='i'
            >{{ option.name }}</option>
          </select>
          <span>{{ helper }}</span>
          <b-form-input
            class='mt-3 mb-3'
            type='number'
            v-model='number'
            autofocus
          ></b-form-input>
          <span>Límite de bits</span>
          <b-form-input class='mt-3' type='number' v-model='limit'></b-form-input>
          <b-button
            class='mt-3 mb-3'
            variant='outline-light'
            type='submit'
            block
            pill
          >Calcular</b-button>
        </b-form>
      </div>
      <div class='col-sm-6 mb-3'>
        <template v-if='this.option.type === "dectobin"'>
          <span>Resultado Signo Magnitud</span>
          <b-form-input class='mt-3' v-model='result[0]'></b-form-input>
          <span>Resultado Complemento a dos</span>
          <b-form-input class='mt-3' v-model='result[1]'></b-form-input>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      decimal: false,
      number: null,
      result: [],
      limit: 5,
      options: [],
      calcType: 0,
      temp: null
    }
  },
  computed: {
    helper() {
      let decimal = this.calcType === 0
      if (decimal) return 'Ingresa un número decimal 🔢'
      else return 'Ingresa un número binario 👨‍💻️'
    },
    option() {
      return this.options[this.calcType]
    }
  },
  methods: {
    calculate() {
      this.result = []
      if (this.option.type === 'dectobin') {
        let enteros = parseInt(this.number)
        let negativo = enteros < 0
        let decimales = 0
        if (this.number.indexOf('.') !== -1) {
          decimales = this.number.substr(this.number.indexOf('.') + 1)
          decimales = parseInt(decimales)
        }
        let result = Math.abs(enteros)
        let code = ''
        const t = true
        while (t) {
          code += result % 2 ? '1' : '0'
          result = parseInt(result / 2)
          if (result === 0) break
        }
        code = code.padEnd(this.limit - 1, '0')
        //signo magnitud
        let sm = code + (negativo ? '1' : '0')
        sm = sm
          .split('')
          .reverse()
          .join('')
        if (sm.length <= this.limit) {
          this.result.push(sm)
        } else {
          this.result.push('Overflow')
        }
        let cm = code.split('')
        cm.push('0')
        cm = cm.reverse()
        let cmf = []
        if (negativo) {
          let found = false
          for (let i = cm.length - 1; i >= 0; i--) {
            if (cm[i] === '1' && !found) {
              found = true
              cmf.push(cm[i])
              continue
            }

            if (found) cmf.push(cm[i] === '1' ? '0' : '1')
            else cmf.push(cm[i])
          }
          cmf = cmf.reverse()
        } else {
          cmf = cm
        }
        if (cmf.length <= this.limit) {
          this.result.push(cmf.join(''))
        } else {
          this.result.push('Overflow')
        }
        this.temp = decimales
      }
    }
  },
  created() {
    this.options.push({ type: 'dectobin', name: 'Decimal 🔢 a Binario 👨‍💻️' })
    this.options.push({ type: 'bintodec', name: 'Binario 👨‍💻️ a Decimal 🔢' })
  }
}
</script>

<style lang="scss" scoped>
#spinner {
  width: 100%;
  margin-bottom: 1rem;
}
</style>