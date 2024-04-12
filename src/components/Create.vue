<template>
  <div style="height: 100%;">
    <div class="input-wrapper">
      <div>제목<input v-model="inputData.title"></div>
      <div>날짜<input type="date" v-model="inputData.date"></div>
      <div>내용<textarea v-model="inputData.content"></textarea></div>
    </div>
    <div>
        <button @click="$router.go(-1)">뒤로가기</button>
        <button @click="clear">초기화</button>
        <button @click="create">생성</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputData: {
        title: '',
        date: null,
        content: ''
      }
    }
  },
  methods: {
    clear() {
      this.inputData = {
        title: '',
        date: null,
        content: ''
      }
    },
    create() {
      if (!this.inputData.title || !this.inputData.date || !this.inputData.content) return
      
      this.$cookies.set(this.$cookies.keys().length + 1, JSON.stringify(this.inputData))
      this.$router.push('/')
    }
  }
}
</script>

<style>
.input-wrapper {
  height: 200px;
  display: inline-grid;
}
</style>
