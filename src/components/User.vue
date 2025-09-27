<script setup>
import {onMounted, ref, watch} from 'vue'

const username = ref('')
const password = ref('')
const enabled = ref('')
const authorities = ref('')
const searchValue = ref('')
const user = ref('')

async function postUser() {
  await fetch('http://localhost:8080/user', {
    method: 'POST',
    headers: {
      'Authorization': 'Basic ' + btoa('admin:password'),
      'Content-Type': 'application/json',
      'Accept': 'application/json',
    },
    body: JSON.stringify({
      username: username.value,
      password: password.value,
      enabled: enabled.value,
      authorities: [authorities.value]
    })
  }).then(function (res) {
    if (res.status !== 200) {
      console.error(res);
    } else {
      res.json().then(function (data) {
        clearForm();
      }.bind(this));
    }
  }.bind(this));
}

function updateUser() {
  if (hasValidInput() && user.value) {
    const i = names.indexOf(user.value)
    names[i] = user.value = `${last.value}, ${first.value}`
  }
}

async function deleteUser() {
  if (user.value !== '') {
    const response = await fetch("http://localhost:8080/user/" + username.value, {
      method: 'DELETE',
      headers: {
        Authorization: 'Basic ' + btoa('admin:password'),
      }
    }).then((res) => {
      if (res.status !== 200) {
        console.error(res);
      } else {
        clearForm();
      }
    })
  }
}

async function getUser() {
  if (searchValue.value !== '') {
    await fetch("http://localhost:8080/user/" + searchValue.value.toString().trim().toLowerCase(), {
      method: 'GET',
      headers: {
        'Authorization': 'Basic ' + btoa('admin:password'),
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
    }).then(function (res) {
      if (res.status === 200) {
        res.json().then(function (data) {
          user.value = data;
        }.bind(this));
      } else if (res.status === 404) {
        alert(`User ${searchValue.value} doesn't exist.`);
      }
      searchValue.value = '';
    }.bind(this));
  }
}

function clearForm() {
  user.value = '';
  username.value = ''
  password.value = ''
  enabled.value = ''
  authorities.value = ''
}

function isSelected() {
  return user.value !== ''
}

function isNewAndValid() {
  return user.value === '' && username.value !== '' && password.value !== '' && authorities.value !== ''
}

watch(user, (user) => {
  username.value = user.username ? username.value = user.username : username.value = ''
  password.value = user.password ? password.value = user.password : password.value = ''
  enabled.value = user.enabled ? enabled.value = user.enabled : enabled.value = ''
  authorities.value = user.authorities ? authorities.value = user.authorities : authorities.value = ''
})

onMounted(() => {
});

</script>

<template>
  <form @submit.prevent="getUser" class="search">
    Use the search bar to find a user by username. The username is case-insensitive.
    <div style="display: flex; align-items: center;">
      <button type="submit">Search</button>
      <input type="text" id="searchValue" v-model="searchValue"/>
    </div>
  </form>

  <form @submit.prevent="postUser" class="details">
    <div style="display: flex; align-items: center;">
      <label for="enabled">Enabled:</label>
      <input type="checkbox" id="enabled" v-model="enabled"/>
    </div>
    <div style="display: flex; align-items: center;">
      <label for="username">Username:</label>
      <input type="text" id="username" v-model="username"/>
    </div>
    <div style="display: flex; align-items: center;">
      <label for="password">Password:</label>
      <input type="text" id="password" v-model="password"/>
    </div>
    <div style="display: flex; align-items: center;">
      <label for="authorities">Authorities:</label>
      <input type="text" id="authorities" v-model="authorities">
    </div>
    <button :disabled="!isNewAndValid()" type="submit">Create</button>
    <button :disabled="!isSelected()" type="button" @click="updateUser">Update</button>
    <button :disabled="!isSelected()" type="button" @click="deleteUser">Delete</button>
    <button :disabled="!isSelected() && !isNewAndValid()" type="button" @click="clearForm">Cancel</button>
  </form>
</template>

<style scoped>
* {
  font-size: inherit;
}


.details {
  clear: both;
  margin-bottom: 1em;
  border: 1px solid #ccc;
}

.details label {
  display: flex;
}

form.details {
  margin-top: 1rem;
}

form.search {
  margin-bottom: 1rem;
}

input#authorities, input#password, input#searchValue, input#username {
  width: -webkit-fill-available;
}

</style>