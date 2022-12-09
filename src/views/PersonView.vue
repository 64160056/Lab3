<script setup lang="ts">
import { ref, reactive, watch } from "vue";
const person = reactive({ name: "", surname: "", gender: "" });
const msg = reactive({ name: "", surname: "", gender: "" });
const cheakName = function (name: string) {
  if (name.trim().length == 0) {
    msg.name = "First name is empty!!!";

    return false;
  }
  msg.name = "";
  return true;
};
const cheakSurname = (surname: string) => {
  if (surname.trim().length == 0) {
    msg.surname = "Second name is empty!!!";

    return false;
  }
  msg.surname = "";
  return true;
};
const cheakGender = (gender: string) => {
  if (gender.trim().length == 0) {
    msg.gender = "Gender is empty!!!";

    return false;
  }
  msg.gender = "";
  return true;
};

const clearFome = function () {
  person.name = "";
  person.surname = "";
  person.gender = "";
  msg.name = "";
  msg.surname = "";
  msg.gender = ""; 
  
};

watch(
  () => person.name,
  (name) => {
    cheakName(name);
  }
);

watch(
  () => person.surname,
  (surname) => {
    cheakSurname(surname);
  }
);
watch(
  () => person.gender,
  (gender) => {
    cheakGender(gender);
  }
);
function doSubmit() {
  if (
    cheakName(person.name) &&
    cheakSurname(person.surname) &&
    cheakGender(person.gender)
  ) {
    console.log(person);
    clearFome();
  }
}
</script>

<template>
  <div>
    <form>
      <label for="name">First Name</label>
      <input type="text" id="name" v-model="person.name" autocomplete="off" />
      <span class="error">{{ msg.name }}</span>

      <label for="surname">Second Name</label>
      <input
        type="text"
        id="surname"
        v-model="person.surname"
        autocomplete="off"
      />
      <span class="error">{{ msg.surname }}</span>

      <label for="gender">Gender</label>
      <select id="gender" v-model="person.gender">
        <option hidden selected>Select Gender</option>
        <option value="M">Male</option>
        <option value="F">Female</option>
      </select>
      <span class="error">{{ msg.gender }}</span>
      <input type="submit" value="Submit" @click.prevent="doSubmit" />
    </form>
    <pre>{{ person }}</pre>
  </div>
</template>

<style scoped>
input[type="text"],
select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid rgb(62, 61, 61);
  border-radius: 6px;
  box-sizing: border-box;
}

input[type="submit"] {
  width: 100%;
  background-color: #4caf50;
  color: rgb(255, 255, 255);
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type="submit"]:hover {
  background-color: #45a049;
}

div {
  border-radius: 5px;
  background-color: #000000;
  padding: 20px;
}

.error {
  color: red;
  font-size: smaller;
  display: block;
}
</style>
