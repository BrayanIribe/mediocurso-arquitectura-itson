<template>
  <div id='calculator'>
    <b-container>
      <div class='block'>👨‍💻️ Calculadora</div>
      <b-form @submit.stop.prevent='calculate' novalidate>
        <div class='row'>
          <div class='offset-sm-2 col-sm-4 mb-3'>
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
          </div>
          <div class='col-sm-4 mb-3'>
            <template v-if='this.option.type === "dectobin"'>
              <p class='mb-3'>Resultado Signo Magnitud</p>
              <b-form-input v-model='result[0]' readonly></b-form-input>
              <p class='mt-3 mb-3'>Resultado Complemento a dos</p>
              <b-form-input v-model='result[1]' readonly></b-form-input>
            </template>
          </div>
          <div class='offset-sm-2 col-sm-8'>
            <b-button
              class='mt-3 mb-3'
              variant='outline-light'
              :disabled='disabled'
              type='submit'
              block
              pill
            >Calcular</b-button>
          </div>
        </div>
      </b-form>
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
      limit: 8,
      options: [],
      calcType: 0,
      running: false,
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
    },
    disabled() {
      return (
        this.running ||
        !this.number ||
        isNaN(parseFloat(this.number)) ||
        this.limit > 17 ||
        Math.abs(this.decimal) > 65534
      )
    }
  },
  methods: {
    pick(i) {
      this.calcType = i
      this.$refs.in.focus()
    },
    dectobin(num) {
      let enteros = parseInt(num)
      let isNegative = num.indexOf('-', 0) !== -1
      let decimales = 0
      if (num.indexOf('.') !== -1) {
        decimales = num.substr(num.indexOf('.') + 1)
        decimales = parseInt(decimales)
      }
      let result = Math.abs(enteros)

      let overflow_sm = false
      let overflow_cm = false

      //signo magnitud
      if (result > Math.pow(2, this.limit - 1) - 1) {
        overflow_sm = true
      }

      if (overflow_sm && !isNegative) {
        overflow_cm = true
      }

      //complemento a dos
      let n = Math.abs(enteros) * -1
      if (
        (isNegative && enteros === 0) ||
        n < Math.pow(2, this.limit - 1) * -1
      ) {
        overflow_cm = true
      }

      let code = []
      let sm = []
      let cm = []
      while (true) {
        code.push(result % 2 ? '1' : '0')
        result = parseInt(result / 2)
        if (result === 0) break
      }
      code = code.reverse()

      return {
        enteros,
        decimales,
        isNegative,
        code,
        overflow_sm,
        overflow_cm
      }
    },
    calculate() {
      if (this.disabled) return
      this.running = true
      this.result = []
      if (this.option.type === 'dectobin') {
        let result = this.dectobin(this.number)
        let sm = result.code
          .join('')
          .padStart(this.limit, '0')
          .split('')
        let cm = sm
        if (result.isNegative) {
          sm[0] = '1'
          let limit = Math.pow(2, this.limit - 1) * -1
          cm = Math.abs(limit + Math.abs(this.number))
          cm = this.dectobin(cm.toString())
            .code.join('')
            .padStart(this.limit, '0')
            .split('')
          cm[0] = '1'
        }

        this.result.push(!result.overflow_sm ? sm.join('') : 'Overflow')
        this.result.push(!result.overflow_cm ? cm.join('') : 'Overflow')

        console.log(
          '%c ✅ Calculator %c\n',
          'background:#35495e ; padding: 1px; border-radius: 3px 0 0 3px;  color: #fff',
          'background: transparent; padding:0;',
          '\nEnteros: ',
          result.enteros,
          '\nDecimales: ',
          result.decimales,
          '\nEs negativo? ',
          result.isNegative ? '✅' : '❌',
          '\nOverflow SM? ',
          result.overflow_sm ? '✅' : '❌',
          '\nOverflow CM? ',
          result.overflow_cm ? '✅' : '❌',
          '\nDec2Bin => ',
          result.code,
          '\nSM => ',
          sm,
          '\nCM => ',
          cm
        )
      }
      this.running = false
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