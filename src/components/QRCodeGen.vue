<template>
  <div class="container">
    <h1 class="title">QR Code Generator</h1>
    <div class="row">
      <div class="left">
        <div class="input-area">
          <span class="label">URL</span>
          <input class="input" v-model="url" placeholder=url>
        </div>
        <div class="input-area">
          <span class="label">Background Color</span>
          <DataPicker @newColor="updateBgColor" :options="options" default="#E9ECEE"/>
        </div>
        <div class="input-area">
          <span class="label">Foreground Color</span>
          <DataPicker @newColor="updateFgColor" :options="options" default="#000000"/>
        </div>
        <div class="input-area">
          <button class="button" @click="generateQrCode">GENERATE</button>
        </div>
      </div>
      <div class="right">
        <p class="result">Your result:</p>
        <img class="qrcode" :src="qrCodeSource" />
        <button class="button download-btn" @click="download">DOWNLOAD</button>
      </div>
    </div>
    </div>
</template>

<script>
import axios from 'axios';
import DataPicker from './DataPicker.vue';

export default {
  name: 'QRCodeGen',
  components: {
    DataPicker,
  },
  data() {
    return {
      url: 'http://annagabriela.github.io',
      backgroundColor: '',
      foregroundColor: '',
      qrCodeSource: '',
      downloadSource: '',
      options: {
        theme: 'nano',

        swatches: [
          'rgb(244, 67, 54)',
          'rgb(233, 30, 99)',
          'rgb(103, 58, 183)',
          'rgb(0, 188, 212)',
          'rgb(0, 150, 136)',
          'rgb(139, 195, 74)',
          'rgb(255, 235, 59)',
        ],

        components: {
          preview: true,
          opacity: false,
          hue: true,

          interaction: {
            input: true,
            save: true,
          },
        },
      },
    };
  },
  mounted() {
    this.generateQrCode();
  },
  methods: {
    updateBgColor(color) {
      this.backgroundColor = color.substring(1);
    },
    updateFgColor(color) {
      this.foregroundColor = color.substring(1);
    },
    generateQrCode() {
      this.qrCodeSource = `http://api.qrserver.com/v1/create-qr-code/?data=${this.url}&color=${this.foregroundColor}&bgcolor=${this.backgroundColor}`;
      axios.get(this.qrCodeSource, { responseType: 'arraybuffer' })
        .then((response) => Buffer.from(response.data, 'binary').toString('base64'))
        .then((response) => { this.downloadSource = `data:image/png;base64,${response}`; });
    },
    download() {
      console.log(this.downloadSource);
      const a = document.createElement('a');
      a.href = this.downloadSource;
      a.download = 'qrcode.png';
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
    },
  },
};
</script>

<style scoped>
.container {
  width: 60%;
  height: 380px;
  position: absolute;
  margin: auto;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: #fff;
  border-radius: 10px;
}

.title {
  margin: 45px auto 0;
  color: #C04646;
}

.row {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.left {
  width: 40%;
  padding: 0 60px;
  text-align: left;
}

.right {
  width: 20%;
  padding: 0 40px;
  text-align: left;
  border-left: 1px solid #D3D3D4;
}

.input-area {
  display: flex;
  align-items: center;
  margin: 30px 0;
}

.input {
  width: 80%;
  padding: .5rem 0.8rem;
  line-height: 1.5;
  border-radius: .3rem;
  border: 1px solid #ced4da;
}

.label {
  font-size: 14px;
  margin: 0 5px 0 0;
}

.button {
  width: 100%;
  padding: 8px;
  border: none;
  border-radius: .3rem;
  background-color: #C04646;
  color: white;
}

.qrcode {
  width: 90%;
}

.result {
  margin: 0 0 10px;
  font-size: 14px;
}

.download-btn {
  width: 90%;
  margin-top: 5px;
}

@media screen and (max-width: 767px) {
  .container {
    width: 90%;
    height: 83%;
  }

  .title { font-size: 22px; }

  .row {
    flex-direction: column;
  }

  .left {
    width: 250px;
    padding: 0;
  }

  .right {
    width: 150px;
    padding: 20px 0;
    border-left: none;
    border-top: 1px solid #D3D3D4;
  }

  .label { font-size: 0.8em; }

  .input { font-size: 0.6em; }
}

</style>
