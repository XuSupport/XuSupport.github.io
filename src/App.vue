<template>
  <div>
    <div class="top-banner">
      <img src="/images/logo.png" alt="图标" class="banner-icon">
      <span class="banner-text">浙江工业大学之江学院网络与信息安全社团</span>
    </div>
    <div class="bg-container">
      <!-- 动态生成气泡 -->
      <div v-for="bubble in bubbles" :key="bubble.id" class="bubble" :style="bubbleStyle(bubble)"></div>

      <!-- 图形部分 -->
<!--      <div-->
<!--          v-for="(image, index) in images"-->
<!--          :key="index"-->
<!--          class="bgBox"-->
<!--          :style="getEdgeStyle()"-->
<!--          v-motion-->
<!--          :initial="{ opacity: 0, y: 100 }"-->
<!--          :enter="{ opacity: 1, y: 0 }"-->
<!--          :duration="1000"-->
<!--      >-->
<!--        <img class="circular-image" :src="image.src" alt="" />-->
<!--      </div>-->
    </div>

    <div class="bgGrid">
      <div class="Grid" v-motion :initial="{ opacity: 0, y: -25 }" :enter="{ opacity: 1, y: 0 }" :duration="1000">
        <div class="mask"></div>
        <div class="itemGrid-row" v-for="i in gridRows" :key="i">
          <div class="itemGrid-cols" v-for="j in gridCols" :key="j"></div>
        </div>
      </div>
    </div>

    <div class="main" v-motion :initial="{ opacity: 0 }" :enter="{ opacity: 1 }" :duration="1000">
      <div class="info">
        <div class="header">
          <img src="/images/logo.png" alt=""/>
        </div>

        <div class="infoText">
          <h1>Hi,</h1>
          <h1>这里是<span class="qn">之江网安社团</span></h1>
        </div>
      </div>

      <div class="typewriter">
        <i class="iconfont icon-baojiaquotation2"></i>
        <VueTyped :strings="typingTexts" :startDelay="300" :typeSpeed="100" :backSpeed="30" :loop="true"
                  :showCursor="true"/>
        <i class="iconfont icon-baojiaquotation"></i>
      </div>

      <div class="btns">
        <a v-for="item in btnList" :key="item.animate" :href="item.href" target="_blank">
          <vs-button type="gradient" :color="item.color" animation-type="scale">
            <i :class="`iconfont ${item.icon}`"></i>
            <template #animate>
              {{ item.animate }}
            </template>
          </vs-button>
        </a>

        <vs-button class="lastBtn" color="#457B9D" animation-type="scale" @click="active = true">
          <i class="iconfont icon-guanyu"></i>
          <template #animate>关于</template>
        </vs-button>
      </div>
    </div>

    <!-- Footer -->
    <div class="footer">By 之江网安社团 | ©2024</div>

    <!-- Dialog -->
    <vs-dialog overlay-blur width="550px" not-center v-model="active">
      <template #header>
        <h2 class="not-margin">关于本站</h2>
      </template>
      <div class="con-content">
        <!-- 技术栈警告框 -->
        <vs-alert color="#FE8599" type="gradient" v-model:visible="techHidden">
          <template #title>技术栈</template>
          <p>本站基于以下技术搭建和以下服务商部署</p>
          <!-- 头像组 -->
          <vs-avatar-group class="aboutAva" float max="8">
            <vs-tooltip placement="top" v-for="item in avaters" :key="item.content">
              <vs-avatar :color="item.color">
                <i :class="`iconfont ${item.icon}`"></i>
              </vs-avatar>
              <template #content>{{ item.content }}</template>
            </vs-tooltip>

            <!-- 固定的 CDN 项目 -->
            <vs-tooltip placement="top">
              <vs-avatar color="#FF6D1A">
                初
              </vs-avatar>
              <template #content>初七云cdn</template>
            </vs-tooltip>
          </vs-avatar-group>
        </vs-alert>

        <!-- 关于社团 -->
        <vs-alert color="#00BCD4" type="gradient" v-model:visible="aboutHidden">
          <template #title>关于社团的章程</template>
          <p>你可以从这里访问 <b>GitHub、哔哩哔哩</b> 以及给团队发 <b>邮件</b> ！</p>
          <p>你可以从这里了解到
            <b><a href="https://planes-cross-5d3.craft.me/ZW734G08dfLdLZ" target="_blank">社团章程</a></b> 、
            <b><a href="https://planes-cross-5d3.craft.me/3Soa2s3sCcHyV1" target="_blank">会议记录</a></b>
            等信息，感谢学长学姐的付出
          </p>
          <p>项目借鉴于：</p>
          <p><a href="https://github.com/QNquenan/homepage-for-vue3"
                target="_blank">https://github.com/QNquenan/homepage-for-vue3</a>
          </p>
        </vs-alert>
      </div>
    </vs-dialog>
  </div>
</template>

<script setup>
import {ref, onMounted} from 'vue'
import {VsNotification} from 'vuesax-alpha'

const techHidden = ref(true)
const aboutHidden = ref(true)
const active = ref(false)
const theme = ref('system')

const images = ref([
  { src: '/images/pic1.jpg' },
  { src: '/images/pic2.jpg' },
  { src: '/images/pic3.jpg' },
  { src: '/images/pic4.jpg' },
])

// 生成气泡数据
const bubbles = ref(Array.from({ length: 20 }, (_, id) => ({
  id,
  size: Math.random() * 50 + 20, // 气泡大小随机 20px - 70px
  left: Math.random() * 100, // 气泡初始水平位置
  duration: Math.random() * 10 + 5, // 上升时长随机 5s - 15s
  delay: Math.random() * 5, // 延迟随机 0s - 5s
})))

const getEdgeStyle = () => {
  const edge = Math.floor(Math.random() * 4);
  const randomOffset = Math.random() * 90;
  switch (edge) {
    case 0: return { top: `${randomOffset}%`, left: '10%' }
    case 1: return { top: `${randomOffset}%`, left: '10%' }
    case 2: return { top: `${randomOffset}%`, left: '10%' }
    case 3: return { top: `${randomOffset}%`, left: '10%' }
  }
}

// 动态气泡样式
const bubbleStyle = (bubble) => ({
  width: `${bubble.size}px`,
  height: `${bubble.size}px`,
  left: `${bubble.left}%`,
  animationDuration: `${bubble.duration}s`,
  animationDelay: `${bubble.delay}s`
})

// Typing text data
const typingTexts = [
  "你好鸭，欢迎来到之江网安资源站！",
  "彼方尚有荣光在，世界不止眼前的苟且，还有诗和远方",
  "累了可以在我这里歇歇脚嗷",
  "May you happy every day"
]

const avaters = ref([
  {
    color: '#43B884',
    icon: 'icon-vue',
    content: 'Vue3'
  },
  {
    color: '#46C93A',
    icon: 'icon-vuesax-linear-vuesax',
    content: 'Vuesax for Vue3'
  },
  {
    color: '#1FD0ED',
    icon: 'icon-less',
    content: 'Less'
  },
  {
    color: '#FFBA00',
    icon: 'icon-vite',
    content: 'Vite'
  },
  {
    color: '#000',
    icon: 'icon-vercel',
    content: 'Vercel'
  },
  {
    color: '#000',
    icon: 'icon-github',
    content: 'Github'
  }
])

// Data for button list
const btnList = [
  {icon: 'icon-github', animate: 'Github', color: '#3d3d3d', href: 'https://github.com/XuSupport'},
  {
    icon: 'icon-bilibili',
    animate: 'BiliBili',
    color: '#0BA6D8',
    href: 'https://b23.tv/D2qCO78'
  },
  {
    icon: 'icon-youjian1',
    animate: 'E-mail',
    color: '#FACB1E',
    href: 'mailto:1102663302@qq.com'
  }

]

// Apply theme
onMounted(() => {
  applyTheme()
})

const applyTheme = () => {
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches
  const themeToApply = theme.value === 'system' ? (prefersDark ? 'dark' : 'light') : theme.value
  document.documentElement.setAttribute('data-theme', themeToApply)
}

</script>


<style lang="less">
@import url(//at.alicdn.com/t/c/font_4685493_lrpbngzgvbk.css);

.bg-container {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
  background: url('/images/3.jpg') no-repeat center center;
  background-size: cover; /* 背景图片自动调整尺寸以覆盖整个容器 */
}




/* 气泡样式 */
.bubble {
  position: absolute;
  bottom: -100px; /* 初始位置在屏幕外底部 */
  background-color: rgba(173, 216, 230, 0.4); /* 淡蓝色气泡 */
  border-radius: 50%;
  animation: floatUp infinite ease-in-out;
}

/* 气泡上升动画 */
@keyframes floatUp {
  0% {
    transform: translateY(0) scale(0.8); /* 初始较小 */
    opacity: 0;
  }
  50% {
    opacity: 0.8;
  }
  100% {
    transform: translateY(-100vh) scale(1.2); /* 缩放并上升到顶部 */
    opacity: 0;
  }
}

.bgBox {
  position: absolute;
  animation: slowMove 10s infinite alternate ease-in-out;
}

.circular-image {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  border: 2px solid rgba(128, 128, 128, 0.3);
}

/* 图形缓慢移动动画 */
@keyframes slowMove {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translateY(-20px) translateX(20px);
  }
}

/* 顶部横幅样式 */
.top-banner {
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed; /* 固定在页面顶部 */
  top: 0;
  left: 0;
  width: 100%; /* 占满整个页面宽度 */
  background-color: white; /* 背景白色 */
  padding: 10px 0; /* 上下内边距 */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* 添加阴影 */
  z-index: 1000; /* 确保横幅显示在其他元素上方 */
}

/* 图标样式 */
.banner-icon {
  width: 24px; /* 图标宽度 */
  height: 24px; /* 图标高度 */
  margin-right: 8px; /* 图标和文字的间距 */
}

/* 文本样式 */
.banner-text {
  font-size: 16px; /* 字体大小 */
  font-weight: bold; /* 粗体 */
  color: gray; /* 文字灰色 */
}


/* 阿里巴巴图标库 */
</style>
