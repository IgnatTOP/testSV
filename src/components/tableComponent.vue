<template>
<div class="container">
  <div>
    <div>
      <input v-model="searchIn" placeholder="Введите имя">
      <button @click="ageIn = !ageIn">Сортировать от 30 лет</button>
      <button @click="sortASC">
  Сортировать по алфавиту {{ sort === 'asc' ? 'по убыванию' : 'по возрастанию' }}
</button>    
</div>
  
    <table>
      <thead>
        <tr>
          <th>Имя</th>
          <th>Возраст</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users_new" :key="user.email">
          <td>{{ user.name }}</td>
          <td>{{ user.age }}</td>
          <td>{{ user.email }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
</template>

<script>
import users from '../assets/users.json';

export default {
  data() {
    return {
      users: users,
      searchIn: '',
      sort: 'asc',
      ageIn: false
    };
  },
  computed: {
    users_new() {
      let users_new = this.users;
      if (this.searchIn) {
        users_new = users_new.filter(user => 
          user.name.toLowerCase().includes(this.searchIn.toLowerCase())
        );
      }
      if (this.ageIn) {
        users_new = users_new.filter(user => user.age >= 30);
      }
      users_new.sort((a, b) => {
        const name1 = a.name.toLowerCase();
        const name2 = b.name.toLowerCase();
        return this.sort === 'asc'
          ? (name1 < name2 ? -1 : name1 > name2 ? 1 : 0)
          : (name1 < name2 ? 1 : name1 > name2 ? -1 : 0);
      });
      return users_new;
    }
  },
  methods: {
    sortAge() {
      this.ageIn = true;
    },
    sortASC() {
      this.sort = this.sort === 'asc' ? 'desc' : 'asc';
    }
  }
};
</script>

<style lang="scss" scoped>
$primary-color: #3e5c76;
$hoverbg: #1d2d44;
$br: 10px;
$padding: 10px 20px;
$input-padding: 5px 10px;

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  div {
    min-width: 800px;
    background: white;
    border-radius: $br;
    padding: 20px;
    div {
      display: flex;
      gap: 20px;
      input {
        padding: $input-padding;
        border: 1px solid #ccc;
        border-radius: $br;
        transition: border-color 0.3s;

        &:focus {
          border-color: $primary-color;
          outline: none;
        }
      }
      button {
        padding: $padding;
        cursor: pointer;
        color: white;
        background-color: $primary-color;
        border: none;
        border-radius: $br;
        transition: background-color 0.3s;
        &:hover {
          background-color: $hoverbg;
        }
      }
    }
    table {
      margin: 20px 0;
      box-shadow: 0px 4px 8px rgba(34, 60, 80, 0.2);
      width: 100%;
      border-radius: $br;
      background-color: white;
      text-align: left;
      th, td {
        padding: 10px 40px;
      }
      thead {
        background: #f2f5f8;
      }
      tbody {
        tr {
          cursor: pointer;
          &:hover {
            background-color: #f2f2f2;
          }
        }
      }
    }
  }
}
</style>
