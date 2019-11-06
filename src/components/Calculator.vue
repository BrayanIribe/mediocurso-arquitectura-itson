<template>
  <div id='calculator'>
    <b-container>
      <div class='block'>👨‍💻️ Calculadora</div>
      <div class='row'>
        <div class='col-sm-4 mb-3 middle'>
          <b-button
            v-for='(option, i) in this.options'
            :key='i'
            @click='pick(i)'
            :variant='calcType === i ? "primary" : "secondary"'
            class='picker'
            block
            pill
          >{{ option.name }}</b-button>
        </div>
        <div class='col-sm-4 mb-3'>
          <b-form @submit.stop.prevent='calculate' novalidate>
            <p>{{ helper }}</p>
            <b-form-input
              class='mt-3 mb-3'
              type='number'
              v-model='number'
              ref='in'
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
        <div class='col-sm-4 mb-3'>
          <template v-if='this.option.type === "dectobin"'>
            <p class='mb-3'>Resultado Signo Magnitud</p>
            <b-form-input v-model='result[0]' readonly></b-form-input>
            <p class='mt-3 mb-3'>Resultado Complemento a dos</p>
            <b-form-input v-model='result[1]' readonly></b-form-input>
          </template>
        </div>
      </div>
    </b-container>
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
    pick(i) {
      this.calcType = i
      this.$refs.in.focus()
    },
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
          cm.push('0')
          cmf = cm
        }
        console.log('SM => ', sm)
        console.log('C2 => ', cmf.join(''))
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
    this.options.push({
      type: 'dectobin',
      name: '🔢👨‍💻️ Decimal  a Binario '
    })
    this.options.push({
      type: 'bintodec',
      name: '👨‍💻️🔢 Binario  a Decimal '
    })
    this.options.push({ type: 'binpbin', name: '➕👨‍💻️ Sumar números binarios' })
  }
}
</script>

<style lang="scss" scoped>
.block,
.center {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.block {
  font-size: 300%;
  font-weight: lighter;
  margin-bottom: 4rem;
  user-select: none;
}
#calculator {
  width: 100%;
}
#spinner {
  width: 100%;
  margin-bottom: 1rem;
}
.picker {
  outline: 0;
  box-shadow: none;
}
.picker.btn-secondary {
  background-color: rgba(0, 0, 0, 0.4) !important;
  color: white !important;
  border: 0px;
}

.middle {
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-direction: column;
}
</style>