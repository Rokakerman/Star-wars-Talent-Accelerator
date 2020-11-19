<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
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
      if(!localStorage.getItem("characters")) {
        console.log("Inside the if")
        const response = await fetch(this.URL, {method: 'GET', headers: {'Content-Type': 'application/json'}})

        this.array = await response.json()  
        console.log(this.array.results)
        localStorage.setItem("characters", JSON.stringify(this.array.results))
        return 
      }
      let storage = localStorage.getItem("characters")
      let list = JSON.parse(storage)
      /*let char = list[0]
      console.log("Outside the if " + char.name)
      const response = await fetch(this.URL, {method: 'GET', headers: {'Content-Type': 'application/json'}})*/
    }
  }
}

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
