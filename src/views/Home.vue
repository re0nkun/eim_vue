<template>
  <div>
    {{ per }} %
    <div class="target" :style="position">
      <a @click="hit" :style="{fontSize: size + 'px'}">
        <div id="target" class="fadeout" :style="{animationDuration: interval + 'ms'}">
          {{ target }}
        </div>
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
      timer_id: null,
      totalCount: 0,
      hitCount: 0
    }
  },
  computed: {
    per () {
      return Math.floor((this.hitCount / this.totalCount) * 10000) / 100
    }
  },
  methods: {
    // modal
    keyAction (e) {
      if (e.keyCode == 27) {
        this.showContent = true
        clearInterval(this.timer_id)
        this.totalCount = 0
        this.hitCount = 0
      }
    },
    closeModal () {
      this.showContent = false

      this.resetAnimation()

      // clearInterval(this.timer_id)
      // this.setPosition()
      // this.totalCount = 0
      // this.hitCount = 0
      this.move()
    },
    setParam (n) {
      this.interval = n.interval
      this.size = n.size

      this.$router.push({ query: { interval: this.interval, size: this.size } })
    },

    // aim
    move () {
      this.totalCount += 1
      this.timer_id = setInterval(() => {
        this.setPosition()
        this.totalCount += 1
      }, this.interval)
    },
    hit () {
      this.resetAnimation()
      clearInterval(this.timer_id)
      this.setPosition()
      this.move()
      this.hitCount += 1
    },
    setPosition () {
      this.position.top = `${this._getRandomNum(0, window.innerHeight - this.size)}px`
      this.position.left = `${this._getRandomNum(0, window.innerWidth - this.size)}px`
    },
    _getRandomNum (min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min)
    },
    resetAnimation () {
      const t = document.querySelector('#target')
      t.className = ""
      window.requestAnimationFrame(function() {
        window.requestAnimationFrame(function() {
          t.className = "fadeout"
        })
      })
      // t.classList.remove('fadeout')
      // void t.offsetWidth
      // t.classList.add('fadeout')
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
@keyframes fadeOut {
  0% {
      transform: scale(1,1);
  }
  100% {
      transform: scale(0,0);
  }
}
.fadeout {
  animation: fadeOut 3s infinite;
}
</style>