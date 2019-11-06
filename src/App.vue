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
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}

#app {
  overflow-y: scroll;
}

body {
  color: white;
  background-color: #e74c3c;
}

#app {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(#34495e, #e74c3c);
  padding-left: 2rem;
  padding-right: 2rem;
}

.form-control {
  background-color: rgba(0, 0, 0, 0.4) !important;
  color: white !important;
  border: 0px;
}
</style>
