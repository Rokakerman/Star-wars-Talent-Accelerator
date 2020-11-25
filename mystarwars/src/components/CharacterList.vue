<template>
  <div>
      <ul @scroll="scrolledToBottom" class="character-list">
          <li v-for="item in parentList" :key="item.name"> 
              <p> {{ item.name }} </p>
          </li>
      </ul>
  </div>
</template>

<script>
export default {
    data() {
        return {

        }
    },
    props: {
        parentList: Array
    },
    methods: {
        loadMore(event) {

        },
        scrolledToBottom(event){
        var el = event.srcElement
        console.log(el.scrollTop + " " + el.scrollHeight + " " + el.clientHeight)
        if(!this.reachedBottom){
            if(el.scrollTop >= (el.scrollHeight - el.clientHeight)-100){
                this.reachedBottom = true
            }
        } 
    },
    }
}
</script>

<style lang="scss" scoped>
.character-list {
    overflow: auto;
    height: 9rem;
    margin: 0px;
    padding: 0px;
}
</style>

