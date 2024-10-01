<script setup>
import { ref } from 'vue'
import VueQrcode from '@chenfengyuan/vue-qrcode';
const url = ref('https://www.google.com')
const imageUrl = ref(new URL('/favicon.ico', import.meta.url).href)
const coverage = ref(0.15)
const onReady = (canvas) => {
  const context = canvas.getContext('2d');
  const image = new Image();
  image.src = imageUrl.value;
  image.onload = () => {
    // 將拿到的圖片畫到 canvas 上
    const width = Math.round(canvas.width * coverage.value);
    const height = Math.round(canvas.height * coverage.value);
    const x = (canvas.width - width) / 2;
    const y = (canvas.height - height) / 2;
    context.drawImage(image, x, y, width, height);
  };
}
</script>

<template>
  <div>
    使用 vue-qrcode 套件 <a href="https://github.com/fengyuanchen/vue-qrcode">參考網址</a><br>

    QRCode 指向： <input type="text" v-model="url">

    <br>
    QRcode 小圖： <input type="text" v-model="imageUrl">  <img :src="imageUrl" alt=""> <br>
    圖片覆蓋率： <input type="number" v-model="coverage">
    <h2>覆蓋</h2>
    <div class="qrcode">
      <VueQrcode :value="url" :options="{ width: 200 }"></VueQrcode>
      <img :src="imageUrl" alt="" class="qrcode__image">
    </div>
  </div>
  <h2>將圖片劃入 QRcode</h2>
  <div class="div">
    <VueQrcode :value="url" :options="{ width: 200 }" @ready="onReady"></VueQrcode>
  </div>
</template>

<style scoped>
.qrcode {
  display: inline-block;
  font-size: 0;
  margin-bottom: 0;
  position: relative;
}

.qrcode__image {
  background-color: #fff;
  border: 0.25rem solid #fff;
  border-radius: 0.25rem;
  box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.25);
  height: 15%;
  left: 50%;
  overflow: hidden;
  position: absolute;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 15%;
}
</style>
