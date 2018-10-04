<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <div class="text-xs-center">
        <img src="/v.png" alt="Vuetify.js" class="mb-5" />
      </div>
      <v-card>
        <v-card-title class="headline">Your users</v-card-title>
        <v-card-text>
          <ul class="items">
            <li
              v-for="(user, index) in users"
              :key="index"
              class="item"
            >
              <div class="item__inner">
                <div class="item__top">
                  <div>
                    <img class="item__avatar" :src="`http://i.pravatar.cc/10${index}`">
                  </div>
                  <h4>{{user.name}}</h4>
                  <span>{{user.email}}</span>
                </div>
                <div class="item__bottom" @click="editUser">
                  <span>Click to Edit</span>
                </div>
              </div>
            </li>
          </ul>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import gql from 'graphql-tag'

export default {
  methods: {
    editUser () {
      console.log('namaste')
    }
  },
  data: () => ({
    users: []
  }),
  apollo: {
    users: gql`query FetchAllUsers{
      users {
        name
        id
        email
      }
    }`
  }
}
</script>


<style scoped>
  .items {
    display: flex;
    flex-wrap: wrap;
    list-style-type: none;
  }


  .item {
    width: 25%;
    padding: 5px;
  }

  .item__inner {
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 8px;
    padding: 5px;
  }

  .item__top {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 0 0 10px;
  }

  .item__bottom {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 5px 0 0 0;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
  }

  .item__avatar {
    height: 100px;
    width: 100px;
    border-radius: 50%;
  }
</style>
