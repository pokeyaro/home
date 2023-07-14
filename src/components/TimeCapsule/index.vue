<template>
  <div class="extension-bar">
    <div class="title">
      <hourglass-full
        theme="two-tone"
        size="24"
        :fill="['#efefef', '#00000020']"
      />
      <span>时光胶囊</span>
    </div>
    <span class="text"
      >今日已经度过了&nbsp;{{ timeData.day.elapsed }}&nbsp;小时</span
    >
    <el-progress
      :text-inside="true"
      :stroke-width="20"
      :percentage="timeData.day.pass"
    />
    <span class="text"
      >本周已经度过了&nbsp;{{ timeData.week.elapsed }}&nbsp;天</span
    >
    <el-progress
      :text-inside="true"
      :stroke-width="20"
      :percentage="timeData.week.pass"
    />
    <span class="text"
      >本月已经度过了&nbsp;{{ timeData.month.elapsed }}&nbsp;天</span
    >
    <el-progress
      :text-inside="true"
      :stroke-width="20"
      :percentage="timeData.month.pass"
    />
    <span class="text"
      >今年已经度过了&nbsp;{{ timeData.year.elapsed }}&nbsp;个月</span
    >
    <el-progress
      :text-inside="true"
      :stroke-width="20"
      :percentage="timeData.year.pass"
    />
    <div v-if="startDate?.length >= 4 && store.siteStartShow">
      <span class="text" v-html="startDateText" />
      <el-progress
        :show-text="false"
        :indeterminate="true"
        :stroke-width="6"
        :percentage="80"
        :duration="3"
      />
    </div>
  </div>
  <br/><br/>
  <div class="extension-bar">
    <div class="title">
      <message-emoji
          theme="two-tone"
          size="24"
          :fill="['#efefef', '#00000020']"
      />
      <span>和优秀的人，做有挑战的事！</span>
    </div>
    <a class="logo">
      <a href="https://www.bytedance.com/" target="_blank">
        <img src="data:image/png;base64,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" alt="ByteDance">
      </a>
      <a :href="recruitment" target="_blank">&nbsp;{{ curYear + 1 }} 校园招聘研发提前批开始啦～</a>
    </a>
    <div class="container">
      <div class="qrcode" style="padding: 24px;">
        <svg shape-rendering="crispEdges" height="120" width="120" viewBox="0 0 33 33"><path fill="#fff" d="M0,0 h33v33H0z"></path><path fill="#000" d="M0 0h7v1H0zM9 0h2v1H9zM12 0h1v1H12zM14 0h1v1H14zM17 0h3v1H17zM21 0h1v1H21zM23 0h2v1H23zM26,0 h7v1H26zM0 1h1v1H0zM6 1h1v1H6zM9 1h2v1H9zM12 1h1v1H12zM14 1h1v1H14zM16 1h1v1H16zM19 1h2v1H19zM22 1h2v1H22zM26 1h1v1H26zM32,1 h1v1H32zM0 2h1v1H0zM2 2h3v1H2zM6 2h1v1H6zM10 2h1v1H10zM14 2h7v1H14zM23 2h1v1H23zM26 2h1v1H26zM28 2h3v1H28zM32,2 h1v1H32zM0 3h1v1H0zM2 3h3v1H2zM6 3h1v1H6zM12 3h2v1H12zM19 3h2v1H19zM22 3h2v1H22zM26 3h1v1H26zM28 3h3v1H28zM32,3 h1v1H32zM0 4h1v1H0zM2 4h3v1H2zM6 4h1v1H6zM8 4h1v1H8zM10 4h2v1H10zM15 4h1v1H15zM17 4h2v1H17zM21 4h4v1H21zM26 4h1v1H26zM28 4h3v1H28zM32,4 h1v1H32zM0 5h1v1H0zM6 5h1v1H6zM9 5h1v1H9zM13 5h1v1H13zM15 5h1v1H15zM17 5h1v1H17zM19 5h3v1H19zM26 5h1v1H26zM32,5 h1v1H32zM0 6h7v1H0zM8 6h1v1H8zM10 6h1v1H10zM12 6h1v1H12zM14 6h1v1H14zM16 6h1v1H16zM18 6h1v1H18zM20 6h1v1H20zM22 6h1v1H22zM24 6h1v1H24zM26,6 h7v1H26zM8 7h1v1H8zM10 7h1v1H10zM12 7h1v1H12zM14 7h1v1H14zM16 7h1v1H16zM22 7h1v1H22zM2 8h2v1H2zM6 8h4v1H6zM12 8h2v1H12zM18 8h1v1H18zM20 8h2v1H20zM25 8h2v1H25zM28 8h1v1H28zM1 9h1v1H1zM3 9h2v1H3zM7 9h7v1H7zM15 9h1v1H15zM17 9h1v1H17zM21 9h6v1H21zM30 9h1v1H30zM32,9 h1v1H32zM5 10h2v1H5zM8 10h2v1H8zM11 10h1v1H11zM15 10h1v1H15zM18 10h1v1H18zM21 10h5v1H21zM31,10 h2v1H31zM1 11h1v1H1zM3 11h3v1H3zM10 11h2v1H10zM13 11h1v1H13zM18 11h2v1H18zM22 11h1v1H22zM24 11h6v1H24zM31,11 h2v1H31zM0 12h1v1H0zM2 12h2v1H2zM6 12h1v1H6zM8 12h2v1H8zM17 12h1v1H17zM19 12h1v1H19zM21 12h1v1H21zM23 12h1v1H23zM25 12h1v1H25zM27 12h1v1H27zM32,12 h1v1H32zM0 13h1v1H0zM2 13h1v1H2zM5 13h1v1H5zM7 13h2v1H7zM10 13h3v1H10zM16 13h3v1H16zM20 13h2v1H20zM24 13h2v1H24zM27 13h1v1H27zM29 13h2v1H29zM2 14h1v1H2zM4 14h3v1H4zM10 14h3v1H10zM14 14h1v1H14zM16 14h1v1H16zM20 14h2v1H20zM24 14h2v1H24zM27 14h1v1H27zM29 14h1v1H29zM0 15h2v1H0zM3 15h3v1H3zM7 15h4v1H7zM14 15h1v1H14zM17 15h3v1H17zM21 15h3v1H21zM25 15h2v1H25zM30 15h2v1H30zM3 16h1v1H3zM5 16h3v1H5zM9 16h5v1H9zM16 16h2v1H16zM19 16h3v1H19zM24 16h3v1H24zM28 16h1v1H28zM30 16h1v1H30zM32,16 h1v1H32zM0 17h1v1H0zM2 17h1v1H2zM5 17h1v1H5zM10 17h1v1H10zM12 17h1v1H12zM14 17h2v1H14zM22 17h2v1H22zM25 17h4v1H25zM30,17 h3v1H30zM0 18h1v1H0zM2 18h1v1H2zM4 18h1v1H4zM6 18h2v1H6zM12 18h1v1H12zM15 18h1v1H15zM18 18h1v1H18zM20 18h3v1H20zM25 18h1v1H25zM27 18h2v1H27zM30 18h2v1H30zM2 19h2v1H2zM7 19h1v1H7zM11 19h1v1H11zM14 19h2v1H14zM19 19h1v1H19zM27 19h2v1H27zM31 19h1v1H31zM2 20h6v1H2zM10 20h2v1H10zM13 20h2v1H13zM16 20h3v1H16zM21 20h1v1H21zM24 20h2v1H24zM29 20h2v1H29zM32,20 h1v1H32zM0 21h2v1H0zM3 21h1v1H3zM5 21h1v1H5zM8 21h2v1H8zM11 21h3v1H11zM15 21h4v1H15zM20 21h2v1H20zM25 21h3v1H25zM29 21h1v1H29zM32,21 h1v1H32zM2 22h2v1H2zM6 22h2v1H6zM10 22h2v1H10zM13 22h2v1H13zM20 22h2v1H20zM23 22h1v1H23zM26 22h1v1H26zM28 22h1v1H28zM30,22 h3v1H30zM1 23h1v1H1zM3 23h1v1H3zM8 23h1v1H8zM13 23h1v1H13zM15 23h2v1H15zM18 23h1v1H18zM24 23h2v1H24zM28 23h2v1H28zM0 24h1v1H0zM2 24h2v1H2zM6 24h3v1H6zM11 24h1v1H11zM15 24h3v1H15zM24 24h5v1H24zM32,24 h1v1H32zM8 25h1v1H8zM12 25h1v1H12zM18 25h1v1H18zM20 25h2v1H20zM23 25h2v1H23zM28 25h1v1H28zM30 25h1v1H30zM0 26h7v1H0zM8 26h1v1H8zM10 26h4v1H10zM16 26h4v1H16zM21 26h1v1H21zM23 26h2v1H23zM26 26h1v1H26zM28 26h3v1H28zM0 27h1v1H0zM6 27h1v1H6zM10 27h1v1H10zM14 27h1v1H14zM17 27h4v1H17zM23 27h2v1H23zM28 27h1v1H28zM30,27 h3v1H30zM0 28h1v1H0zM2 28h3v1H2zM6 28h1v1H6zM10 28h3v1H10zM16 28h2v1H16zM20 28h1v1H20zM22 28h10v1H22zM0 29h1v1H0zM2 29h3v1H2zM6 29h1v1H6zM8 29h1v1H8zM11 29h4v1H11zM16 29h1v1H16zM20 29h1v1H20zM22 29h3v1H22zM27 29h1v1H27zM29 29h1v1H29zM31,29 h2v1H31zM0 30h1v1H0zM2 30h3v1H2zM6 30h1v1H6zM8 30h5v1H8zM14 30h1v1H14zM18 30h1v1H18zM20 30h4v1H20zM25 30h2v1H25zM29 30h2v1H29zM0 31h1v1H0zM6 31h1v1H6zM10 31h4v1H10zM20 31h2v1H20zM23 31h4v1H23zM32,31 h1v1H32zM0 32h7v1H0zM10 32h1v1H10zM12 32h7v1H12zM20 32h4v1H20zM25 32h1v1H25zM27 32h4v1H27z"></path></svg>
      </div>
      <div class="video">
        <video ref="videoElement" height="120" width="213.33" autoplay>
          <source :src="videoSource" type="video/mp4">
        </video>
      </div>
    </div>
    <span>扫码即可内推，或点击投递链接：<a class="link" :href="referral" target="_blank">{{ referral }}</a></span>
  </div>
</template>

<script setup>
import { onMounted, onBeforeUnmount, ref } from "vue";
import { HourglassFull, MessageEmoji } from "@icon-park/vue-next";
import { getTimeCapsule, siteDateStatistics } from "@/utils/getTime.js";
import { mainStore } from "@/store";
const store = mainStore();

// 进度条数据
const timeData = ref(getTimeCapsule());
const startDate = ref(import.meta.env.VITE_SITE_START);
const startDateText = ref(null);
const timeInterval = ref(null);

const videoElement = ref(null);
const videoSource = ref(import.meta.env.VITE_COMPANY_VIDEO);

onMounted(() => {
  timeInterval.value = setInterval(() => {
    timeData.value = getTimeCapsule();
    if (startDate.value)
      startDateText.value = siteDateStatistics(new Date(startDate.value));
  }, 1000);
});

onBeforeUnmount(() => {
  clearInterval(timeInterval.value);
});

// 内推消息
const curYear = new Date().getFullYear();
const recruitment = ref(import.meta.env.VITE_RECRUIT_NOTICE);
const referral = ref(import.meta.env.VITE_REFERRAL_LINK);
</script>

<style lang="scss" scoped>
.extension-bar {
  width: 100%;
  .title {
    display: flex;
    flex-direction: row;
    align-items: center;
    margin: 0.2rem 0 1.5rem;
    font-size: 1.1rem;
    .i-icon {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-right: 6px;
    }
  }
  .text {
    display: block;
    margin: 1rem 0rem 0.5rem 0rem;
    font-size: 0.95rem;
  }
}
.logo {
  img {
    width: 85px;
  }
}
.container {
  display: flex;
}
.qrcode,
.video {
  flex: 0;
  padding: 24px;
}
</style>
