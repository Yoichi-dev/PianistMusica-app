<template>
  <div id="ponArea" @click="add()">
    画面をタップ！<br />
    （音量注意）
    <audio
      id="pontyaka"
      :src="require('@/assets/audio/pon.mp3').default"
      loop
    ></audio>
    <div id="hiddenFlg" class="poncounter">
      <img
        v-if="hundredFlg"
        width="70px"
        class="countUp"
        :src="require('@/assets/image/counter/' + hundred + '.png')"
      />
      <img
        v-if="tenFlg"
        width="70px"
        class="countUp"
        :src="require('@/assets/image/counter/' + ten + '.png')"
      />
      <img
        v-if="oneFlg"
        id="one"
        width="70px"
        class="countUp"
        :src="require('@/assets/image/counter/' + one + '.png')"
      />
    </div>
    <div class="poncounterimg">
      <div id="ponimg"></div>
    </div>
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/PONmujika_NonLoop.gif')"
    />
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/counter/0.png')"
    />
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/counter/1.png')"
    />
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/counter/2.png')"
    />
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/counter/3.png')"
    />
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/counter/4.png')"
    />
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/counter/5.png')"
    />
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/counter/6.png')"
    />
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/counter/7.png')"
    />
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/counter/8.png')"
    />
    <img
      class="hid"
      width="1px"
      :src="require('@/assets/image/counter/9.png')"
    />
  </div>
</template>

<script>
import { gsap } from 'gsap'

export default {
  name: 'PonPage',
  data() {
    return {
      hundred: 0,
      ten: 0,
      one: 0,
      hundredFlg: false,
      tenFlg: false,
      oneFlg: false,
      pon: 0,
    }
  },
  head: {
    title: 'ぽんカウンター',
    meta: [
      {
        hid: 'og:site_name',
        property: 'og:site_name',
        content: 'ぽんカウンター | 画面タップで「ぽん」がカウントされます',
      },
      {
        hid: 'og:title',
        property: 'og:title',
        content: 'ぽんカウンター | 画面タップで「ぽん」がカウントされます',
      },
      {
        hid: 'og:url',
        property: 'og:url',
        content: 'https://pianistmusica.net',
      },
      {
        hid: 'description',
        name: 'description',
        content: 'ぽんカウンター | 画面タップで「ぽん」がカウントされます',
      },
      {
        hid: 'og:image',
        property: 'og:image',
        content: 'https://pianistmusica.net/og.jpg?20220214',
      },
      {
        name: 'twitter:card',
        property: 'twitter:card',
        content: 'summary_large_image',
      },
      {
        name: 'twitter:creator',
        property: 'twitter:creator',
        content: '@PianistMusica',
      },
      {
        name: 'twitter:site',
        property: 'twitter:site',
        content: '@PianistMusica',
      },
      { name: 'format-detection', content: 'telephone=no' },
    ],
  },
  methods: {
    add() {
      if (this.pon === 0) {
        document.getElementById('pontyaka').play()
      }
      this.pon++
      this.countImg(String(this.pon))
      this.execImg(this.pon)
    },
    countImg(count) {
      if (count.length === 1) {
        this.oneFlg = true
        this.one = count
      } else if (count.length === 2) {
        this.tenFlg = true
        if (this.ten !== count.charAt(count.length - 2)) {
          this.ten = count.charAt(count.length - 2)
        }
        this.one = count.slice(-1)
      } else if (count.length === 3) {
        this.hundredFlg = true
        if (this.hundred !== count.charAt(count.length - 3)) {
          this.hundred = count.charAt(count.length - 3)
        }
        if (this.ten !== count.charAt(count.length - 2)) {
          this.ten = count.charAt(count.length - 2)
        }
        this.one = count.slice(-1)
      }
    },
    execImg(count) {
      const id = `pon_${count}`
      const ponImgElement = document.createElement('img')
      ponImgElement.src = require(`@/assets/image/PONmujika_NonLoop.gif`)
      ponImgElement.src = ponImgElement.src + `?id=${count}`
      ponImgElement.setAttribute('id', id)
      ponImgElement.style.position = 'absolute'
      ponImgElement.style.width = `300px`
      ponImgElement.style.zIndex = 999
      ponImgElement.style.opacity = 0
      document.getElementById('ponimg').append(ponImgElement)
      gsap.to(`#${id}`, {
        keyframes: {
          '0%': { x: -50, y: 0, opacity: 1 },
          '50%': { x: -100, y: -40, opacity: 1 },
          '100%': { x: -150, y: -10, opacity: 1 },
          easeEach: 'none',
        },
        duration: 2,
        onComplete: () => {
          document.getElementById(id).remove()
        },
      })
    },
  },
}
</script>

<style>
body {
  margin: 0 !important;
}

#ponArea {
  height: 100vh;
  width: 100vw;
}

.hid {
  display: none;
}

.fadeout {
  animation: fadein-keyframes 2.5s ease 0s 1 forwards;
}

@keyframes fadein-keyframes {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

.poncounterimg {
  margin-top: 40vh;
  text-align: center;
  width: 100vw;
  position: absolute;
  z-index: 999;
}

.poncounter {
  text-align: center;
  width: 100vw;
  margin-top: 20vh;
  position: absolute;
  z-index: 999;
}
</style>