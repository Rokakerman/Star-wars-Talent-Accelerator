<template>
  <div>
      <ul>
          <li v-for="item in parentList" :key="item.name"> 
              <p> {{ item.name }} </p>
          </li>
      </ul>
  </div>
</template>

<script>
export default {
    data() {
        return {

        }
    },
    props: {
        parentList: Array
    }
}
</script>

<style>

</style>