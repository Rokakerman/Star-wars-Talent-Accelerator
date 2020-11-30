<template>
  <div class="list-container">
      <ul @scroll="handleScroll">
          <li v-for="item in parentList" :key="item.name"> 
              <p> {{ item.name }} </p>
          </li>
      </ul>
      <button class="load" @click="loadMore"> </button>
  </div>
</template>

<script>
export default {
    data() {
        return {
            hasScrolledToBottom: false
        }
    },
    props: {
        parentList: Array
    },
    methods: {
        handleScroll: function(event) {
            /*if((event.target.offsetHeight + event.target.scrollTop) >= event.target.scrollHeight) {
                this.hasScrolledToBottom = true
                return this.$emit("loadMore")
            } else {
                this.hasScrolledToBottom = false
            }*/
        },
        loadMore() {
            return this.$emit("loadMore")
        }
    }
}
</script>

<style lang="sass" scoped>

.list-container
    display: flex
    flex-direction: column
    justify-content: center
    align-items: center

ul 
    overflow: auto
    height: 15rem
    list-style: none
    margin: 0px
    padding: 0px
    border: 2px solid white
    width: 50%

.load
    margin: 0px
    padding: 0px
    height: 1rem
    width: 3rem
    
</style>
