<script setup lang="ts"></script>
<template>
  <div>
    <form>
      <label for="name">First Name</label>
      <input type="text" id="name" placeholder="Please enter First name" />

      <label for="surname">Second Name</label>
      <input type="text" id="surname" placeholder="Please enter Second name" />

      <label for="gender">Gender</label>
      <select id="gender">
        <option value="M">Male</option>
        <option value="F">FeMale</option>
      </select>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>

<style scoped>
input[type="text"],
select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid rgb(0, 0, 0);
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
</style>
