<template>
    <div class="search-container">
        <div class="input-row">
            <div class="search-button-container" @click="filterMe">
                <!--<button class="search-button" @click="filterMe">  filter </button> -->
            </div>
            <input class="input-field" type="text" v-model="param">
        </div>
        <div class="filter-row">
            <button @click="firstNameFilter" v-bind:class="{ 'active': firstName, 'filter': !firstName }"> <h3> First Name </h3> </button>
            <button @click="lastNameFilter" v-bind:class="{ 'active': lastName, 'filter': !lastName }"> <h3> Last Name </h3> </button>
        </div>
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
            lastName: false,
            nextUrl: ''
        }
    },
    methods: {
        async filterMe() {
            /* 
                This function sends a fetch request with the user param
                and retrieves all object's who's name property matched the given param.
                Since the API uses pagination this function also checks if there is a next 
                pagination link availible and executes code acordingly, then fires whatever
                optional and additional filter the user chose with the search
            */

            const response = await fetch(this.URL + this.param, {method: 'GET', headers: {'Content-Type': 'application/json'}})
            let readable = await response.json()
            this.nextUrl = readable.next
            let firstMatch = readable.results
            let y = 1

            for(let x = 0; x < y; x++) {
                if(!this.nextUrl) {
                    console.log('There are no pages left')
                } else {
                    const newResponse = await fetch(this.nextUrl, {method: 'GET', headers: {'Content-Type': 'application/json'}})
                    let newReadable = await newResponse.json()
                    this.nextUrl = newReadable.next
                    firstMatch = firstMatch.concat(newReadable.results)
                    y++
                }
            }
            // This is the list of the raw fetch results, hence the name of the variable
            this.firstMatchArray = firstMatch
            
            /*if(!this.nextUrl) {
                //Do the regular case here
                console.log("If its empty")

            } else {
                console.log("If its not empty")
                const response = await fetch(this.nextUrl, {method: 'GET', headers: {'Content-Type': 'application/json'}})
                let newReadable = await response.json()
                this.nextUrl = newReadable.next
                let newArray = this.firstMatchArray.concat(readable.results)
                this.firstMatchArray = newArray
                    
            }*/
            
            return this.checkFilter()
        },
        firstNameFilter() {
            // Firstname filter toggle
            this.firstName = !this.firstName
            return this.lastName = false
        },
        lastNameFilter() {
            // Lastname filter toggle
            this.lastName = !this.lastName
            return this.firstName = false
        },
        checkFilter() {
            // This function checks if any of the filters have been selected
            if (this.firstName === true) {
                // Do this
                console.log("in the if")
                return this.getFirstNames()
            } else if (this.lastName === true) {
                // Do this 
                console.log("in the else")
                return this.getLastNames()
            }
            console.log('invoking')
            return this.invoke()
        },
        async getFirstNames() {
            console.log('Your in firstName function now')
            let criteria = this.param
            let filteredList = this.firstMatchArray.filter(el => {
                const [firstname, lastname] = el.name.toLowerCase().split(' ')
                return firstname.includes(criteria.toLowerCase())
            });
            
            let sortedList = filteredList.sort((a, b) => a.name.localeCompare(b.name, 'es', { sensitivity: 'base' }))
            filteredList.forEach(el => console.log(el.name))
            this.filteredArray = sortedList

            return this.invoke()
        },
        async getLastNames() {
            this.firstMatchArray.forEach(el => console.log(el.name))
            console.log('Your in lasttName function now')
            let criteria = this.param
            let filteredList = this.firstMatchArray.filter(el => {
                let index = el.name.lastIndexOf(' ')

                if (index == -1) {
                    index = 0
                }
                //const [firstname, lastname] = el.name.toLowerCase().split(index)  
                return el.name.toLowerCase().slice(index).includes(criteria.toLowerCase())

            });
            filteredList.forEach(element => console.log(element.name))
            let sortedList = filteredList.sort((a, b) => a.name.localeCompare(b.name, 'es', { sensitivity: 'base' }))
            //sortedList.forEach(el => console.log(el.name))
            this.filteredArray = sortedList

            return this.invoke()
        },
        invoke() {
            if (this.lastName == false && this.firstName == false) {
                let sortedList = this.firstMatchArray.sort((a, b) => a.name.localeCompare(b.name, 'es', { sensitivity: 'base' }))
                sortedList.forEach(el => console.log(el.name))
                return this.$emit("filter", sortedList)
            } else {
                return this.$emit("filter", this.filteredArray)
            }
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

<style lang="sass" scoped>
.active
    color: black
    border: yellow 1px solid
    height: 2rem
    width: 45%
    font-weight: bolder
    font-size: 15px
    border-radius: 3px
    background-color: yellow

.search-container
    display: flex
    flex-direction: column
    margin: 0px
    padding: 0px
    width: 80%
    max-width: 25rem
    height: 100%
    justify-content: space-around
    align-items: center

.input-row
    width: 100%
    display: flex
    justify-content: center
    
.search-button-container
    border: 2px solid yellow
    border-top: 0px
    border-left: 0px
    border-right: 0px
    background-image: url('~@/assets/search.svg')
    width: 15%
    background-repeat: no-repeat
    background-position: center

.input-field
    border: yellow 2px solid
    background: none
    border-top: 0px
    border-right: 0px
    color: white
    font-weight: bold
    font-size: 18px
    width: 85%
    height: 2rem

.filter-row
    width: 100%
    max-width: 30rem    
    display: flex
    justify-content: space-between

.filter
    color: yellow
    background-color: black
    border: yellow 1px solid
    height: 2.5rem
    width: 45%
    font-weight: bolder
    font-size: 15px
    border-radius: 3px

h3
    margin: 0px
    padding: 0px
</style>