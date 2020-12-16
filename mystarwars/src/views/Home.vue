<template>
  <div class="home">
    <div class="x"> </div>
    <div class="triangle"> </div>
    <div class="y"> </div>
    <header class="home-header">  
      <article class="preview-container" v-if="showObjectPreview">
          <header>
              <h2> <!-- Name of character --> {{ objectInPreview.name }} </h2>
              <h4> <!-- Name of homeworld --> {{ getHomeWorld }} </h4>
          </header>
          <main>

          </main>
          <footer>
              <h3> Films </h3>
              <ul>
                  <li></li>
              </ul>
          </footer>
      </article>
    </header>
    <main>
      <Search
        v-on:filter="filtered"
      />
    </main>
    <footer>
    <CharacterList 
      :parentList="array"
      v-on:loadMore="nextPage"
      v-on:previewToggled="showPreview"
    />
    </footer>
  </div>
</template>

<script>
// @ is an alias to /src
import CharacterList from '@/components/CharacterList.vue'
import Jedis from '@/assets/jedis.jpg'
import Search from '@/components/Search.vue'

export default {
  name: 'Home',
  components: { CharacterList, Search },
  data() {
    return {
      error: null,
      URL: "https://swapi.dev/api/people/",
      next: '',
      array: [],
      image: { backgroundImage: "url(@/assets/)" },
      showObjectPreview: false,
      objectInPreview: '',
      objectHomeWorld: '',
      objectFilms: ''
    }
  },
  computed: {
    /*async getHomeWorld() {
      if (!this.objectInPreview) {
        return 
      } else {
        const URL = this.objectInPreview.homeworld
        const response = await fetch(URL, {method: 'GET', headers: {'Content-Type': 'application/json'}})
        let readable = await response.json()
        console.log(readable.name)
        let name = readable.name
      }
      return name
    }*/
  },
  created() {
    this.fetchData()
  
  },
  methods: {
    async fetchData () {
      /*if(!sessionStorage.getItem("characters")) {
        console.log("Inside the if")
        const response = await fetch(this.URL, {method: 'GET', headers: {'Content-Type': 'application/json'}})
        let readable = await response.json()
        this.array = readable.results

        console.log(this.array)
        sessionStorage.setItem("characters", JSON.stringify(this.array))
        return 
      }
      //console.log("outside the if")
      let storage = sessionStorage.getItem("characters")
      let readable = JSON.parse(storage)*/

      const response = await fetch(this.URL, {method: 'GET', headers: {'Content-Type': 'application/json'}})
      let readable = await response.json()
      this.array = readable.results

      return this.next = readable.next
      /*let char = list[0]
      console.log("Outside the if " + char.name)
      const response = await fetch(this.URL, {method: 'GET', headers: {'Content-Type': 'application/json'}})*/
    },
    filterMe() {
      //let criteria = this.;
      let results = this.array.filter(el => {
        // This filters out the firstname and returns any matched with the critera
        const [firstname, lastname] = el.name.toLowerCase().split('');
        return firstname.includes(criteria.toLowerCase());
      })    
      
    },
    async nextPage() {
      console.log("Nextpage method called")
      if(this.next == null) {
        //throw new TypeError('There is more content to load')
        return console.log('There are no more pages to load')
      }

      const response = await fetch(this.next, {method: 'GET', headers: {'Content-Type': 'application/json'}})
      let readable = await response.json()
      let newArray = this.array.concat(readable.results)
      console.log(newArray)
      this.array = this.array.concat(readable.results)
      return this.next = readable.next
    },
    filtered(filteredList) {
      console.log(filteredList)
      this.next = null
      return this.array = filteredList
    },
    async showPreview(param) {
      this.objectInPreview = param
      this.showObjectPreview = true
      console.log(param)
      return
    },
    async getHomeWorld() {
      const URL = this.objectInPreview.homeworld
      const response = await fetch(URL, {method: 'GET', headers: {'Content-Type': 'application/json'}})
      let readable = await response.json()
      console.log(readable.name)
      let name = readable.name
      return this.objectHomeWorld = name
    }
  }
}
</script>

<style lang="sass" scoped>
.home 
  display: flex
  flex-direction: column
  justify-content: center
  align-items: center

header 
  height: 45vh
  background: transparent

.triangle 
  clip-path: polygon(100% 0, 0 50%, 100% 100%)
  background-color: red
  background-image: url('~@/assets/5.jpg')
  height: 70%
  width: 100%
  z-index: -1
  position: fixed
  background-size: cover
  background-repeat: no-repeat
  border: solid white 10px
  top: 0px

.x 
  clip-path: polygon(100% 0, 0 0, 0 100%)
  background-color: green
  background-image: url('~@/assets/mellan.jpg')
  height: 35%
  width: 100%
  z-index: -1
  position: fixed
  background-size: cover
  background-repeat: no-repeat
  border: solid white 10px
  top: 0px

.y 
  clip-path: polygon(0 1%, 100% 54%, 100% 100%, 0% 100%)
  background-color: yellow
  background-image: url('~@/assets/warpspeed.jpg')
  background-position: center
  height: 66%
  width: 100%
  z-index: -1
  position: fixed
  bottom: 0rem
  background-size: cover
  background-repeat: none
  border: solid white 10px
  border-bottom: 0px

.home-header
  display: flex
  justify-content: center
  align-items: center

main 
  height: 10vh
  background: transparent
  width: 100vw
  display: flex
  justify-content: center
  align-items: center

.preview-container
    border: 1px solid green
    width: 90%
    height: 90%
  

footer 
  height: 45vh
  background: transparent
  width: 90%
  display: flex
  justify-content: center
  align-items: center

</style>