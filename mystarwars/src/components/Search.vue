<template>
    <div>
        <button @click="filterMe">  filter </button>
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
            /*const response = await fetch(this.URL + this.param , {method: 'GET', headers: {'Content-Type': 'application/json'}})
            let readable = await response.json()
            let people = readable.results
            let firstName = await people.filter(el => el.name.split(' ').slice(0, -1).join(' '))
            console.log(firstName)
            this.array = readable*/

            const response = await fetch('http://swapi.dev/api/people/?page=1', {method: 'GET', headers: {'Content-Type': 'application/json'}})
            let x = await response.json()
            let list = x.results
            console.log(list)

            

            /*let firstName = fullName.split(' ').slice(0, -1).join(' ');
            let lastName = fullName.split(' ').slice(-1).join(' ');*/

            //let criteria = " o";
            //let results = this.array.filter(el => el.name.toLowerCase().includes(criteria.toLowerCase()))
            //console.log(results)
        }
    }
}
</script>

<style>

</style>