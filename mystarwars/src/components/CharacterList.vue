<template>
  <div class="list-container">
      <!-- <article class="preview-container" v-if="showObjectPreview">
          <header>
              <h2> </h2>
              <h4> </h4>
          </header>
          <main>

          </main>
          <footer>
              <h3> Films </h3>
              <ul>
                  <li></li>
              </ul>
          </footer>
      </article> -->
      <ul @scroll="handleScroll">
          <li class="list-item" v-for="(item, index) in parentList" :key="index"> 
              <p @click="getObject(index)"> {{ item.name }} </p> 
          </li>
      </ul>
      <button class="load" @click="loadMore"> </button>
  </div>
</template>

<script>
export default {
    data() {
        return {
            hasScrolledToBottom: false,
            objectInPreview: Object,
            showObjectPreview: false
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
        },
        getObject(param) {
            console.log(param)
            let object = this.parentList[param]
            this.objectInPreview = object
            this.showObjectPreview = true
            return this.$emit("previewToggled", this.objectInPreview)
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
    width: 80%

c

ul 
    overflow: auto
    height: 15rem
    list-style: none
    margin: 0px
    padding: 0px
    border: none
    width: 100%

.list-item
    width: 100%
    height: 3rem
    border: 1px solid yellow
    border-top: 0px
    border-right: 0px
    border-left: 0px
    display: flex
    justify-content: flex-start
    align-items: center
    margin: 0px
    padding: 0px

p
    color: white
    margin: 0px 0px 0px 1rem
    font-weight: 900
    font-size: 20px
    &:hover
        cursor: pointer

.load
    margin: 0px
    padding: 0px
    height: 1rem
    width: 3rem
    
</style>
