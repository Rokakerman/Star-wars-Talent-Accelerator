<template>
  <div class="home">
    <div class="x"> </div>
    <div class="triangle"> </div>
    <div class="y"> </div>
    <header>  </header>
    <main>
      <Search
        v-on:filter="filtered"
      />
    </main>
    <footer>
    <CharacterList 
      :parentList="array"
      v-on:loadMore="nextPage"
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
      image: { backgroundImage: "url(@/assets/)" }
    }
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
    }
  }
}
</script>

<style scoped>
header {
  height: 50vh;
  background: transparent;
}
.triangle {
clip-path: polygon(100% 0, 0 50%, 100% 100%);
background-color: red;
background-image: url('~@/assets/5.jpg');
height: 70%;
width: 100%;
z-index: -1;
position: fixed;
background-size: cover;
background-repeat: no-repeat;
border: solid white 10px
}

.x {
  clip-path: polygon(100% 0, 0 0, 0 100%);
  background-color: green;
  background-image: url('~@/assets/mellan.jpg');
  height: 35%;
  width: 100%;
  z-index: -1;
  position: fixed;
  background-size: cover;
background-repeat: no-repeat;
border: solid white 10px
}

.y {
  clip-path: polygon(0 1%, 100% 54%, 100% 100%, 0% 100%);
  background-color: yellow;
  background-image: url('~@/assets/warpspeed.jpg');
  background-position: center;
  height: 66%;
  width: 100%;
  z-index: -1;
  position: fixed;
  bottom: 0rem;
  background-size: cover;
  background-repeat: none;
  border: solid white 10px;
  border-bottom: 0px;
}

main {
  height: 10vh;
  background: transparent
}

footer {
  height: 40vh;
  background: transparent
}
</style>