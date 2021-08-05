<template>
  <div id="app">
    <button v-on:click="recognize">recognize</button>
    <img
      id="text-img"
      alt="Vue logo"
      src="./assets/testocr.png"
    >
    <img
      id="img"
      alt="Vue logo"
      src="./assets/orc.jpg"
    >
    <img
      id="test"
      alt="Vue logo"
      src="./assets/test.png"
    >
  </div>
</template>

<script>
/* eslint-disable */
import { createWorker, PSM, OEM } from "tesseract.js";
const worker = createWorker({
  logger: m => {}, //console.log(m)
  // 配置本地资源路径，语言包的实际位置是通过langPath+'/'+lang+'.traineddata.gz'得到的，所以不要改语言包文件名
  langPath: "./tesseract/"
});

export default {
  name: "app",
  data() {
    return {};
  },
  methods: {
    async recognize() {
      console.time();
      const img = document.getElementById("img");
      await worker.load();
      await worker.loadLanguage("chi_sim"); //可以通过'+'设置多个语言
      await worker.initialize("chi_sim", OEM.LSTM_ONLY);
      await worker.setParameters({
        tessedit_pageseg_mode: PSM.SINGLE_BLOCK
      });
      const {
        data: { text }
      } = await worker.recognize(img);
      console.log(text);
      console.timeEnd();
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
