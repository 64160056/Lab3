<script setup lang="ts">
import { ref, reactive, watch, nextTick } from "vue";
class Person {
  id: number;
  name: string;
  surname: string;
  gender: string;
  static lastId = 1;
  constructor(name: string, surname: string, gender: string) {
    this.id = Person.lastId++;
    this.name = name;
    this.surname = surname;
    this.gender = gender;
  }
}
const person = reactive<Person>({ id: -1, name: "", surname: "", gender: "" });
const msg = reactive({ name: "", surname: "", gender: "" });
const personList = ref<Person[]>([]);
const showform = ref(false);
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

const clearForm = function () {
  person.name = "";
  person.surname = "";
  person.gender = "";
  //Updating
  nextTick(() => {
    //Update form finish
    msg.name = "";
    msg.surname = "";
    msg.gender = "";
  });
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
    if (person.id < 0) {
      const p = new Person(person.name, person.surname, person.gender);
      personList.value.push(p);
    } else {
      const index = personList.value.findIndex((item) => item.id === person.id);
      personList.value[index].name = person.name;
      personList.value[index].surname = person.surname;
      personList.value[index].gender = person.gender;
    }
    clearForm();
    showform.value = false;
  }
}
const doEdit = function (p: Person) {
  person.name = p.name;
  person.surname = p.surname;
  person.gender = p.gender;
  person.id = p.id;
  showform.value = true;
};
const doDelete = function (p: Person) {
  const index = personList.value.findIndex((item) => item.id === p.id);
  personList.value.splice(index, 1);
};
</script>

<template>
  <div v-if="showform" class="form">
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
  </div>
  <div v-if="!showform">
    <div style="width: 100%; text-align: right">
      <button style="width: 100px" @click="showform = !showform">Add</button>
    </div>
    <table id="persons">
      <thead>
        <th>ID</th>
        <th>Frist Name</th>
        <th>Second Name</th>
        <th>Gender</th>
        <th style="width: 250px">Operator</th>
      </thead>
      <tr v-for="(item, index) in personList" :key="index">
        <td>{{ item.id }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.surname }}</td>
        <td>{{ item.gender }}</td>
        <td>
          <button
            style="width: 100px; background-color: grey"
            @click="doEdit(item)"
          >
            Edit
          </button>
          <button
            style="width: 100px; margin-left: 1em; background-color: red"
            @click="doDelete(item)"
          >
            Delete
          </button>
        </td>
      </tr>
      <tr>
        <td
          colspan="5"
          style="text-align: center"
          v-if="personList.length === 0"
        >
          No Data
        </td>
      </tr>
    </table>
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

input[type="submit"],
button {
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

div.form {
  border-radius: 5px;
  background-color: #000000;
  padding: 20px;
}

.error {
  color: red;
  font-size: smaller;
  display: block;
}
#persons {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#persons td,
#persons th {
  border: 1px solid rgb(191, 191, 191);
  padding: 8px;
}

#persons tr:nth-child() {
  background-color: #6e90a0;
}

#persons tr:hover {
  background-color: rgb(101, 150, 88);
}

#persons th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04aa6d;
  color: rgb(255, 255, 255);
}
</style>
