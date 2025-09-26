<script setup>
import {onMounted, reactive, ref, watch} from 'vue'

const users = reactive([]);
const selected = ref('')
const username = ref('')
const password = ref('')
const enabled = ref('')
const authorities = ref('')

const fetchUsers = async () => {
  try {
    const headers = new Headers();
    headers.append('Authorization', 'Basic ' + btoa('admin:password'));
    const response = await fetch('http://localhost:8080/users', {headers: headers});
    const data = await response.json()
    data.forEach(user => {
      users.push({
        username: user.username,
        password: user.password,
        enabled: user.enabled,
        authorities: [user.authorities]
      })
    })
  } catch (error) {
    console.error('Error fetching users:', error)
  }
  return users;
}

watch(selected, (user) => {
  username.value = user.username ? username.value = user.username : username.value = ''
  password.value = user.password ? password.value = user.password : password.value = ''
  enabled.value = user.enabled ? enabled.value = user.enabled : enabled.value = ''
  authorities.value = user.authorities ? authorities.value = user.authorities : authorities.value = ''
})


function create() {
  if (isNewAndValid()) {
    users.push({
      username: username.value.toString().toLowerCase().trim(),
      password: password.value,
      enabled: enabled.value,
      authorities: [authorities.value]
    })
    selected.value = users[users.length - 1]
  }
}

function update() {
  if (hasValidInput() && selected.value) {
    const i = names.indexOf(selected.value)
    names[i] = selected.value = `${last.value}, ${first.value}`
  }
}

function del() {
  if (selected.value) {
    const i = names.indexOf(selected.value)
    names.splice(i, 1)
    selected.value = first.value = last.value = ''
  }
}

function clear() {
  selected.value = '';
  username.value = ''
  password.value = ''
  enabled.value = ''
  authorities.value = ''
}

function isSelected() {
  return selected.value !== ''
}

function isNewAndValid() {
  return selected.value === '' && username.value !== '' && password.value !== '' && enabled.value !== '' && authorities.value !== ''
}

onMounted(() => {
  fetchUsers()
});

</script>

<template>
  <div class="user">
    Select a user to edit or enter user info to create a new user.
    <select size="5" v-model="selected">
      <option v-for="user in users" :key="user.username" :value="user">{{ user.username }}</option>
    </select>

    <div class="details">
      <label>Enabled: <input type="checkbox" v-model="enabled"></label>
      <label>Username: <input v-model="username"></label>
      <label>Password: <input v-model="password"></label>
      <label>Authorities: <input v-model="authorities"></label>
    </div>

    <div class="buttons">
      <button :disabled="!isNewAndValid()" @click="create">Create</button>
      <button :disabled="!isSelected()" @click="update">Update</button>
      <button :disabled="!isSelected()" @click="del">Delete</button>
      <button :disabled="!isSelected() && !isNewAndValid()" @click="clear">Cancel</button>
    </div>
  </div>
</template>

<style scoped>
* {
  font-size: inherit;
}

.user {
  display: grid;
  grid-template-columns: 100%;
}

.details {
  clear: both;
  margin-bottom: 1em;
  border: 1px solid #ccc;
}

.details label {
  display: flex;
  margin: .5em;
  padding: .5em;
}

.details label input {
  width: 100%;
}

select {
  float: left;
  margin: 0 1em 1em 0;
  width: 14em;
}

.buttons {
  clear: both;
}

button + button {
  margin-left: 5px;
}

</style>