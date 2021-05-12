<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <h1>Video Summary Converter</h1>
          <p>32933 Research Project - Autumn 2021</p>
          <p>Supervisor: Professor Wei Liu</p>
          <ul >
            <li>Wencheng Xiong - 13680436</li>
            <li>Xiaorong Pan-12765982</li>
            <li>Yali Zhu - 13336704</li>
          </ul>
        </div>
      </div>
    </div>
    <Panel />
  </div>
</template>

<script>
import Panel from './components/Panel.vue';

export default {
  name: 'App',
  components: {
    Panel,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
ul{
  list-style: none;
}
</style>
