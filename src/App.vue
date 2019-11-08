<template>
  <div id='app'>
    <div class='c-container'>
      <Calculator></Calculator>
    </div>
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
#app,
.c-container {
  width: 100%;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}

.c-container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

@media screen and (max-width: 600px) {
  .c-container {
    height: auto;
  }
}

#app {
  overflow-y: scroll;
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
  padding-left: 2rem;
  padding-right: 2rem;
}

.form-control {
  background-color: rgba(0, 0, 0, 0.4) !important;
  color: white !important;
  border: 0px;
}
</style>
