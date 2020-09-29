<template>
  <div>
    <div id="target" class="target" :style="position">
      <a @click="hit" :style="{fontSize: size + 'px'}">
        <div>{{ target }}</div>
      </a>
    </div>

    <openModal v-show="showContent" @close-modal="closeModal" @set-param="setParam" />
  </div>
</template>

<script>
import openModal from '@/components/openModal'
export default {
  components: { 
    openModal 
  },
  data () {
    return {
      position: {
        top: 0,
        left: 0,
      },
      interval: this.$route.query.interval || 3000,
      size: this.$route.query.size || 50,
      showContent: false,
      target: '💩',
      timer_id: null
    }
  },
  methods: {
    // modal
    keyAction(e) {
      if (e.keyCode == 27) {
        this.showContent = true
      }
    },
    closeModal () {
      this.showContent = false

      clearInterval(this.timer_id)
      // this.setPosition()
      this.timer_id = setInterval(() => {
        this.setPosition()
      }, this.interval)
    },
    setParam (n) {
      this.interval = n.interval
      this.size = n.size

      this.$router.push({ query: { interval: this.interval, size: this.size } })
    },

    // aim
    move () {
      this.timer_id = setInterval(() => {
        this.setPosition()
      }, this.interval)
    },
    hit () {
      // const t = document.querySelector('#target')
      // t.className = "target"
      // window.requestAnimationFrame(function() {
      //   window.requestAnimationFrame(function() {
      //     t.className = "target fadeout"
      //   })
      // })
      // t.classList.remove('fadeout')
      // void t.offsetWidth
      // t.classList.add('fadeout')
      clearInterval(this.timer_id)
      this.setPosition()
      this.timer_id = setInterval(() => {
        this.setPosition()
      }, this.interval)
    },
    setPosition () {
      this.position.top = `${this._getRandomNum(0, window.innerHeight - this.size)}px`
      this.position.left = `${this._getRandomNum(0, window.innerWidth - this.size)}px`
    },
    _getRandomNum (min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min)
    }
  },
  created() {
    //キーコードによる動作の登録
    window.addEventListener("keydown", this.keyAction)
  },
  beforeDestroy() {
    //キーコードによる動作の削除
    window.removeEventListener("keydown", this.keyAction)
  },
  mounted() {
    this.setPosition()
    this.move()
  },
}
</script>

<style scoped>
.target {
  position: fixed;
}
.target a {
  cursor: pointer;
  user-select: none;
  /* font-size: 80px; */
}
/* @keyframes fadeOut {
  0% {
      transform: scale(1,1);
  }
  100% {
      transform: scale(0,0);
  }
}
.fadeout {
  animation: fadeOut 3s infinite;
} */
</style>