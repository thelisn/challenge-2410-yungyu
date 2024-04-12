<template>
  <div>
      <search-list></search-list>
      <button @click="gotoCreate">새로 만들기</button>
  </div>
</template>

<script>
import SearchList from './SearchList.vue'

export default {
  components: { SearchList },
  data() {
    return {
      isShow: false,
    }
  },
  methods: {
    gotoCreate() {
      this.$router.push('/create')
    }
  }
}
</script>
<style>

</style>
