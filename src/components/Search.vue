<template>
  <div class="search">
    <ul>
      <li v-for="(item, index) in list" :key="index" @click="handleClick(item)" >
        <a href="#" @mouseover="handleMouseover(item)" @mouseout="handleMouseout">{{item.title}}</a>
      </li>      
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    list: Array
  },
  mounted() {
    console.log(this.list)
  },
  methods: {
    handleMouseover(item) {
      this.$emit('hover', item)
    },
    handleMouseout(item) {
      this.$emit('mouseOut', item)
    },
    handleClick(item) {
      this.$emit('clickItem', item)
    }
  }
}
</script>

<style>
.search {
  position: absolute;
  left: 10px;
  top: 10px;
  bottom: 10px;
  z-index: 999;
  width: 300px;
  /* height: 100%; */
  overflow: scroll;;
}
ul {
  background-color: #fff;
  margin: 0;
  padding: 10px;
}
li {
  list-style: none;
  text-align: left;
  margin-bottom: 10px;
}
li:hover {
  color: red;
}
</style>