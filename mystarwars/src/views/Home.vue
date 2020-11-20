<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <CharacterList :parentList="array"/>
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
    window.addEventListener('beforeunload', this.fetchData())
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
      let storage = sessionStorage.getItem("characters")
      let list = JSON.parse(storage)
      this.array = list.results
      return
      /*let char = list[0]
      console.log("Outside the if " + char.name)
      const response = await fetch(this.URL, {method: 'GET', headers: {'Content-Type': 'application/json'}})*/
    }
  }
}
</script>
