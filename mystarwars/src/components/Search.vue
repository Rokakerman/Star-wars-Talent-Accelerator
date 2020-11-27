<template>
    <div>
        <button @click="filterMe">  filter </button>
        <input type="text" v-model="param">
        <button @click="firstName = true"> First Name </button>
        <button @click="firstName = false"> Last Name </button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            firstMatchArray: '',
            filteredArray: '',
            URL: 'https://swapi.dev/api/people/?search=',
            param: '',
            firstName: false,
            nextUrl: ''
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
            let y = 1
            let page = 1

            const response = await fetch(this.URL + this.param, {method: 'GET', headers: {'Content-Type': 'application/json'}})
            let readable = await response.json()
            this.nextUrl = readable.next
            let firstMatch = readable.results
            console.log(firstMatch)
            this.firstMatchArray = firstMatch
            
            for(let x = 0; x < y; x++ ) {
                if(!this.nextUrl) {
                    //Do the regular case here
                    console.log("If its empty")

                } else {
                    console.log("If its not empty")
                    const response = await fetch(this.nextUrl, {method: 'GET', headers: {'Content-Type': 'application/json'}})
                    let readable = await response.json()
                    this.nextUrl = readable.next
                    let newArray = this.firstMatchArray.concat(readable.results)
                    this.firstMatchArray = newArray
                    y++
                }
            }
            

            /*let firstName = fullName.split(' ').slice(0, -1).join(' ');
            let lastName = fullName.split(' ').slice(-1).join(' ');*/

       
            return this.firsNames()
            //let criteria = " o";
            //let results = this.array.filter(el => el.name.toLowerCase().includes(criteria.toLowerCase()))
            //console.log(results)
        },
        async firsNames() {
            console.log('Your in firstName function now')
            let criteria = this.param
            let filteredList = this.firstMatchArray.filter(el => {
                const [firstname, lastname] = el.name.toLowerCase().split(' ')
                return firstname.includes(criteria.toLowerCase())
            });
            for(let x = 0; x < filteredList.length; x++) {
                console.log(filteredList[x].name)
            }
            let sortedList = filteredList.sort()
            console.log(await sortedList)
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