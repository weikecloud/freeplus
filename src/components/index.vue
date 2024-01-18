<template>
  <n-global-style />
  <n-grid x-gap="12" :cols="1">
    <n-gi>
      <div class="light-green" id="content">
        <!-- <h1>标题标题</h1>
        <br>
        <p>☆ Plus专车，国内可用无需楼梯Plus专车，国内可用无需楼梯</p>
        <p>☆ Plus专车，国内可用无需楼梯Plus专车，国内可用无需楼梯Plus专车，国内可用</p> -->
      </div>
    </n-gi>
  </n-grid>
  <n-divider>GPT 3.5 免费频道</n-divider>
  <n-grid x-gap="12" y-gap="12" cols="2 400:4 600:6">
    <n-grid-item class="cardclss" v-for="item in itemsfree" :key="item.carID">
      <n-card :title="item.carID" @click="redirectTo(item.carID)">
        <img class="plusicon"
             :src="'https://img.closeai.biz/endpoint?url='+baseUrl+'%2Fendpoint%3Fcarid%3D' + item.carID">
      </n-card>
    </n-grid-item>
  </n-grid>

  <n-divider>GPT4.0 付费频道</n-divider>
  <n-grid x-gap="12" y-gap="12" cols="2 400:4 600:6">
    <n-grid-item class="cardclss" v-for="item in itemsplus" :key="item.carID">
      <n-card :title="item.carID" @click="redirectTo(item.carID)">
        <img class="plusicon"
             :src="'https://img.closeai.biz/endpoint?url='+baseUrl+'%2Fendpoint%3Fcarid%3D' + item.carID">
      </n-card>
    </n-grid-item>
  </n-grid>


  <n-divider>国内镜像</n-divider>
  <n-grid x-gap="12" y-gap="12" cols="2 400:4 600:6">

    <n-grid-item class="cardclss" v-for="item in chatimages" :key="item.title">
      <n-card :title="item.title" @click="redirectToimages(item.url)">
        <img class="plusicon"
             src="https://img.closeai.biz/endpoint?url=https%3A%2F%2Ffree-gpt.club%2Fendpoint%3Fcarid%3DFree-GPT-003">
      </n-card>

    </n-grid-item>
  </n-grid>

</template>

<script lang="ts">
import axios from 'axios';
export default {
  data() {
    return {
      baseUrl:window.location.origin,
      itemsfree: [],
      itemsplus: [],
      chatimages: [{
        'title': 'ChatGPT-001',
        'url': 'https://chat.freegpts.org/login'
      }, {
        'title': 'ChatGPT-002',
        'url': 'https://chat.freegpts.org/login'
      },]
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      axios.post(`${this.baseUrl}/carpage`, {
        page:1,
        size: 1000
      })
          .then(response => {
            console.log(response);
            const allItems = response.data.data.list;
            document.getElementById('content').innerHTML = response.data.notice;
            // this.notice = response.data.data.notice
            this.itemsfree = allItems.filter(item => item.isPlus === 0);
            this.itemsplus = allItems.filter(item => item.isPlus === 1);
          })
          .catch(error => {
            console.error('请求错误:', error);
          });
    },
    redirectTo(carID) {
      const url = `${this.baseUrl}/auth/login?carid=${carID}`;
      window.location.href = url;
    },
    redirectToimages(url) {
      window.location.href = url;
    },
  }
};

</script>