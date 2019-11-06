<template>
  <div id='app'>
    <Calculator></Calculator>
  </div>
</template>

<script>
import Calculator from './components/Calculator'
export default {
  name: 'app',
  components: { Calculator }
}
</script>

<style lang="scss">
html,
body,
#app {
  width: 100%;
  height: 100%;
  min-height: 100%;
}

body {
  color: white;
  background-color: #e74c3c;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(#34495e, #e74c3c);
}

.form-control {
  background-color: rgba(0, 0, 0, 0.4) !important;
  color: white !important;
  border: 0px;
}
</style>
