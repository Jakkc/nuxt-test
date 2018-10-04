<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <v-card>
        <v-card-title class="headline">Your users</v-card-title>
        <v-card-text>
          <ul class="items">
            <li
              v-for="(user, index) in users"
              :key="index"
              class="item"
              @click="editUser(user)"
            >
              <div class="item__inner">
                <div class="item__top">
                  <div>
                    <img class="item__avatar" :src="`http://i.pravatar.cc/10${index}`">
                  </div>
                  <h4>{{user.name}}</h4>
                  <em>{{user.company.name}}</em>
                  <span class="item__email">{{user.email}}</span>
                </div>
                <div class="item__bottom">
                  <span>Click to Edit</span>
                </div>
              </div>
            </li>
          </ul>
        </v-card-text>
      </v-card>
    </v-flex>
    <div class="modal" v-if="isEditing">
      <div class="modal__inner">
        <p>Edit details for {{values.name}}</p>
        <form @submit.prevent>
          <fieldset>
            <label>Name</label>
            <input v-model="values.name" />
          </fieldset>
          <fieldset>
            <label>Email</label>
            <input v-model="values.email" />
          </fieldset>
          <fieldset>
            <label>Company</label>
            <input v-model="values.company" />
          </fieldset>
          <fieldset>
            <button class="button" @click="dispatchUserMutation">Save</button>
          </fieldset>
        </form>
      </div>
    </div>
  </v-layout>
</template>

<script>
import gql from 'graphql-tag'

export default {
  name: 'WorkingComponent',
  methods: {
    editUser (user) {
      this.isEditing = true
      this.values.name = user.name
      this.values.email = user.email
      this.values.company = user.company.name
      this.values.id = user.id
    },
    dispatchUserMutation () {
      this.$apollo.mutate({
        mutation: gql`
          mutation updateUser($id: ID, $user: UserInput) {
            updateUser(id: $id, user:$user) {
              id
              name
            }
          }
        `,
        variables: {
          id: this.values.id,
          user: {
            name: this.values.name
          }
        },
        update: (store, data) => {
          console.log('nearly there...')
        }
      })
        .then(r => {
          console.log('done')
          this.isEditing = false
        })
        .catch(e => console.error(e))
    }
  },
  data: () => ({
    users: [],
    isEditing: false,
    values: {
      name: '',
      email: '',
      company: ''
    }
  }),
  apollo: {
    users: gql`query FetchAllUsers{
      users {
        name
        id
        email
        company {
          name
        }
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
    cursor: pointer;
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
    border-radius: 0 0 4px 4px;
    background-color: rgba(222,124,121, 0.2);
    border-top: 1px solid rgba(255, 255, 255, 0.4);
  }

  .item__email {
    font-size: 11px;
  }

  .item__avatar {
    height: 100px;
    width: 100px;
    border-radius: 50%;
  }

.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.2);
}

.modal__inner {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  height: 80%;
  background-color: white;
  border-radius: 8px;
  color: black;
  padding: 40px;
  overflow: scroll;
}
fieldset {
  display: flex;
  border: none;
  margin: 0 0 30px;
  min-width: 500px;
}
input {
  border: 1px solid #222;
  border-radius: 8px;
  height: 48px;
  width: 100%;
  padding: 0 0 0 10px;
}
label {
  margin: 0 0 5px;
}

button {
  padding: 7px;
  background-color: rgba(222,124,121, 0.2);
  border-radius: 4px;
  min-width: 120px;
  border: 1px solid #222;
  transition: background-color 0.1s ease-in;
}

button:hover {
  background-color: rgba(222,124,121, 0.5);
}
</style>
