<template>
  <div class="home">
    <header> <img class="triangle"> </header>
    <main>
    <button @click="filterMe">  filter </button>
    </main>
    <footer>
    <CharacterList :parentList="array"/>
    </footer>
  </div>
</template>

<script>
// @ is an alias to /src
import CharacterList from '@/components/CharacterList.vue'

export default {
  name: 'Home',
  components: { CharacterList },
  data() {
    return {
      error: null,
      URL: "https://swapi.dev/api/people/",
      array: []
    }
  },
  created() {
    this.fetchData()
  
  },
  methods: {
    async fetchData () {
      if(!sessionStorage.getItem("characters")) {
        console.log("Inside the if")
        const response = await fetch(this.URL, {method: 'GET', headers: {'Content-Type': 'application/json'}})
        let readable = await response.json()
        this.array = readable.results

        console.log(this.array)
        sessionStorage.setItem("characters", JSON.stringify(this.array))
        return 
      }
      console.log("outside the if")
      let storage = sessionStorage.getItem("characters")
      let readable = JSON.parse(storage)

      this.array = readable
      return
      /*let char = list[0]
      console.log("Outside the if " + char.name)
      const response = await fetch(this.URL, {method: 'GET', headers: {'Content-Type': 'application/json'}})*/
    },
    filterMe() {
      let criteria = " o";
      let results = this.array.filter(el => el.name.toLowerCase().includes(criteria.toLowerCase()))
      console.log(results)
    }
  }
}
</script>

<style scoped>
header {
  height: 50vh;
}
.triangle {
clip-path: polygon(100% 0, 0 50%, 100% 100%);
background-color: red;
height: 100%;
width: 100%;
}

main {
  height: 10vh;
}

footer {
  height: 40vh;
}
</style>