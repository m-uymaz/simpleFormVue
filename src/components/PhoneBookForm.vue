<template>
  <h1>{{ msg }}</h1>

  <div @keydown.enter="postFunc" class="card">
    <label for="firstName">First name:
      <br>
      <input 
      v-model="fields.firstName" id="firstName" type="text"></label> 
    <br>
    <label for="lastName">Last name:
      <br>
      <input 
      v-model="fields.lastName" id="lastName" type="text"></label>
    <br>
    <label for="phoneNumber">Phone number:
      <br>
      <input
      v-model="fields.phoneNumber"
      id="phoneNumber"></label>
      <br>
      <label v-if="invalidFields" class="invalid-fields">{{ invalidFieldsMsg }}</label> 
    <br>
    <br>
    <button @click="postFunc" type="button">Post</button>
  </div>

  <div class="table-div" v-if="phoneBook.length">
    <table>
      <thead>
        <th>First name</th>
        <th>Last name</th>
        <th>Phone number</th>
      </thead>
      <tr v-for="contact of phoneBook">
        <td>{{ contact.firstName }}</td>
        <td>{{ contact.lastName }}</td>
        <td>{{ contact.phoneNumber }}</td>
      </tr>
    </table>
  </div>
</template>

<script setup lang="ts">
type PhoneBook = {
  firstName: string,
  lastName: string,
  phoneNumber: string
}

type Fields = {
  firstName: string,
  lastName: string,
  phoneNumber: string
}

import { Ref, ref } from 'vue'

defineProps<{ msg: string}>()

const phoneBook: Ref<PhoneBook[]> | []= ref([]);

const invalidFields: Ref<boolean> = ref(false);
const invalidFieldsMsg: Ref<string> = ref('');

const fields: Ref<Fields> = ref({
  firstName: '',
  lastName: '',
  phoneNumber: ''
})

const verifyNumber = () => {
  if(!Number(fields.value.phoneNumber)) {
    invalidFields.value = true;
  } else if(fields.value.phoneNumber.length < 10) {
    invalidFields.value = true;
  }

  invalidFields.value ? invalidFieldsMsg.value = 'Phone number is not valid!' : '';
}

const postFunc = () => {
  if(!fields.value.firstName || !fields.value.lastName || !fields.value.phoneNumber) {
    invalidFields.value = true;
    invalidFieldsMsg.value = 'Invalid fields';
  } else {
    invalidFields.value = false;
    verifyNumber();
  };

  if(invalidFields.value) return;

  phoneBook.value.push({
    firstName: fields.value.firstName,
    lastName: fields.value.lastName,
    phoneNumber: fields.value.phoneNumber
  });

  phoneBook.value.sort((a, b) => a.lastName.localeCompare(b.lastName));

  fields.value = {
    firstName: '',
    lastName: '',
    phoneNumber: ''
  }
}

</script>

<style scoped>
.read-the-docs {
  color: #888;
}

.table-div {
  text-align: -webkit-center;
}

.invalid-fields {
  color: red;
}

th, tr, td {
  padding: 0 2px 0 2px;
  text-align: center;
  border: 1px solid black;
}
</style>
