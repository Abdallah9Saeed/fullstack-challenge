<script>
import UserCard from "@/components/UserCard.vue";
export default {
  data: () => ({
    apiResponse: null,
    usersList:[],
  }),
  components:{UserCard},
  created() {
    this.fetchData()
  },

  methods: {
    async fetchData() {
      const url = 'http://localhost:8002'
      this.apiResponse = await (await fetch(url)).json()
      if(this.apiResponse.users){
        this.usersList=this.apiResponse.users;
      }
    }
  }
}
</script>

<template>
  <div v-if="!apiResponse">
    Pinging the api...
  </div>

  <div v-if="apiResponse">

    <UserCard v-for="user in usersList" :key="user.id" :user="user" />


  </div>
</template>