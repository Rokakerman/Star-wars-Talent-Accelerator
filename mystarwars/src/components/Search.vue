<template>
    <div>
        <button @click="filterMe">  filter </button>
        <input type="text" v-model="param">
    </div>
</template>

<script>
export default {
    data() {
        return {
            array: '',
            URL: 'https://swapi.dev/api/people/?search=',
            param: '',
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

       
            return this.firsNames()
            //let criteria = " o";
            //let results = this.array.filter(el => el.name.toLowerCase().includes(criteria.toLowerCase()))
            //console.log(results)
        },
        async firsNames() {
            let criteria = this.param
            let filteredList = this.array.filter(el => {
                const [firstname, lastname] = el.name.toLowerCase().split(' ')
                return firstname.includes(criteria.toLowerCase())
            });
            for(let x = 0; x < filteredList.length; x++) {
                console.log(filteredList[x].name)
            }
            console.log(await filteredList)
        }
    },
    computed: {
        /*firstNames: function() {
            this.array.filter(el => {
                const [firstname, lastname] = el.name.toLowerCase().split(' ')
                return firstname.includes(criteria.toLowerCase())
            });
        },
        lastNames: function() {
            this.array.filter(el => {
                const [firstname, lastname] = el.name.toLowerCase().split(' ')
                return lastname.includes(criteria.toLowerCase())
            });
        }*/
    }
}
</script>

<style>

</style>