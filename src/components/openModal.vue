<template>
  <div>
    <div id="overlay" @click="closeModal">
      <div id="content" @click.stop>
        <p>
          間隔：<input type="number" min="0" step="1" v-model="interval">
          サイズ：<input type="number" min="0" step="1" v-model="fontSize">
        </p>
        <p><button @click="setParam">save</button></p>
        <p><button @click="closeModal">close</button></p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      interval: this.$route.query.interval || 3000,
      fontSize: this.$route.query.size || 50,
    }
  },
  methods: {
    closeModal () {
      this.$emit('close-modal')
    },
    setParam () {
      this.$emit('set-param', { interval: this.interval, size: this.fontSize })
      this.$emit('close-modal')
    }
  }
}
</script>

<style scoped>
#overlay {
  z-index: 1;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}
#content {
  z-index: 2;
  width:50%;
  padding: 1em;
  background-color: #fff;
}
</style>