<template>
    <div>
        <button @keyup="filterMe">  filter </button>
        <input type="text" v-model="param" @keyup="filterMe">
    </div>
</template>

<script>
export default {
    data() {
        return {
            array: '',
            URL: 'https://swapi.dev/api/people/?search=',
            param: ''
        }
    },
    methods: {
        async filterMe() {
            const response = await fetch(this.URL + this.param , {method: 'GET', headers: {'Content-Type': 'application/json'}})
            let readable = await response.json()
            this.array = readable
            console.log(readable)

            //let criteria = " o";
            //let results = this.array.filter(el => el.name.toLowerCase().includes(criteria.toLowerCase()))
            //console.log(results)
        }
    }
}
</script>

<style>

</style>