<template>
  <div v-if="isShow">
    <div class="search-wrapper">
      <p>현재 조회 방식 : {{ dataMethod }}</p>
      <button @click="switchDataMethod">switch</button>
    </div>
    <p style="color: red;">{{ errorMessage }}</p>
    <div class="search-wrapper">
      <div v-if="!switchSearchMethod">
        <input v-model="searchKeyword" @input="searchByKeyword">
      </div>
      <div v-else>
        <input type="date" v-model="startDate" @change="searchByDate">
        <input type="date" v-model="endDate" @change="searchByDate">
      </div>
      <button @click="switchSearch">switch</button>
    </div>

    <div style="margin-bottom: 50px;" v-for="item in list" :key="item.title">
      {{ `${item.title} (${item.date}) - ${item.content}` }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'SearchList',
  data() {
    return {
      list: [],
      isShow: false,
      switchSearchMethod: false,
      searchKeyword: '',
      startDate: null,
      endDate: null,
      dataMethod: 'cookies',
      errorMessage: '',
    }
  },
  mounted() {
    this.isShow = true
    this.getAllContents()
  },
  methods: {
    getAllContents() {
      this.list = []
      this.errorMessage = ''
      if (this.dataMethod === 'cookies') {
          this.$nextTick(() => 
          this.$cookies.keys().forEach(key => {
            this.list.push($cookies.get(key))
          })
        )
      } else { 
          this.$http.get('/api/list')
          .then((res) => {
            this.list = res.data;
          })
          .catch((err) => {
            console.log(err)
            this.errorMessage = 'server closed'
          })
        }
    },
    switchSearch() {
      this.getAllContents()
      this.searchKeyword = ''
      this.startDate = null
      this.endDate = null
      this.switchSearchMethod = !this.switchSearchMethod
    },
    switchDataMethod() {
      this.dataMethod = this.dataMethod === 'cookies' ? 'axios' : 'cookies';
      this.getAllContents()
    },
    searchByKeyword() {
      if(this.searchKeyword === '') return this.getAllContents()

      this.list = []
      const keys = this.$cookies.keys()

      keys.forEach(key => {
        if (this.$cookies.get(key).title.includes(this.searchKeyword) || this.$cookies.get(key).content.includes(this.searchKeyword)) this.list.push(this.$cookies.get(key))
      })
    },
    searchByDate() {
      if (!this.startDate || !this.endDate) return this.getAllContents()

      this.list = []
      const keys = this.$cookies.keys()

      keys.forEach(key => {
        if (this.startDate <= this.$cookies.get(key).date && this.$cookies.get(key).date <= this.endDate) this.list.push(this.$cookies.get(key))
      })
    },
  }
}
</script>

<style>
.search-wrapper {
  margin-bottom: 50px;
  display: flex;
  justify-content: center;
}
</style>