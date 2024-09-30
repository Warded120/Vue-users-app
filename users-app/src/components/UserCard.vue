<template>
  <div>
    <div class="toolbar">
      <button @click="filterGender('all')">All</button>
      <button @click="filterGender('Male')">Male</button>
      <button @click="filterGender('Female')">Female</button>
    </div>

    <div>
      <div class="container" v-if="filteredUsers.length > 0">
        <div
          v-for="(userData, index) in filteredUsers"
          :key="index"
          :class="['user-card', userData.age >= 18 ? 'adult' : 'child']"
        >
          <img :src="userData.photo" alt="User photo" class="user-photo" />
          <h2>{{ userData.firstName }} {{ userData.lastName }}</h2>
          <p>Gender: {{ userData.gender }}</p>
          <p v-if="userData.age >= 18">Age: {{ userData.age }}</p>
          <p>Position: {{ userData.position }}</p>
          <h3>Hobbies</h3>
          <ul>
            <li v-for="(hobby, index) in userData.hobbies" :key="index">
              {{ hobby }}
            </li>
          </ul>
        </div>
      </div>
      <p v-else class="container empty">Список юзерів пустий</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from "vue";
import { User } from "@/models/User";

export default defineComponent({
  name: "UserCard",
  setup() {
    const userArr = createUsers();
    const users = reactive(userArr);
    const filteredUsers = ref([...users]);
    const selectedGender = ref("all");

    const filterGender = (gender: string) => {
      selectedGender.value = gender;
      if (gender === "all") {
        filteredUsers.value = users;
      } else {
        filteredUsers.value = users.filter((user) => user.gender === gender);
      }
    };

    return {
      filteredUsers,
      filterGender,
    };
  },
});

function createUsers(): User[] {
  const users: User[] = [];

  for (let i = 0; i < 10; i++) {
    const user = new User(
      `FirstName${i}`,
      `LastName${i}`,
      i % 2 === 0 ? "Male" : "Female",
      15 + i,
      `Position${i}`,
      ["Reading", "Gaming", "Sports"]
    );
    users.push(user);
  }

  return users;
}
</script>

<style scoped>
.container {
  display: flex;
  flex-wrap: wrap;
}

.toolbar {
  display: flex;
  gap: 10px;
  margin: 10px;
}

.toolbar button {
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.toolbar button:hover {
  background-color: #0056b3;
}

.user-card {
  border: 2px solid black;
  padding: 10px;
  width: 30%; /* Ширина картки */
  margin: 10px;
  border-radius: 10px;
  text-align: center;
}

.adult {
  background-color: lightgreen;
}

.child {
  background-color: lightcoral;
}

.user-photo {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 10px;
}

.empty {
  margin: 10px;
}
</style>
