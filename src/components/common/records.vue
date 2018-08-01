<template>
  <div>
    <div class="userlist-item" :key="item.id" v-for="item in lists">
      <p>{{item.typename}}</p>
      <span>{{item.time}}</span>
      <em class="color-red">{{item.amount}}</em>
    </div>
  </div>
</template>

<script>
export default {
  name: 'records',
  props: ['lists']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.slider{height:3rem;overflow:hidden;}
.slider img{height:100%;width:100%;display:block;}
</style>
