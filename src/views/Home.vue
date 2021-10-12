<template>
  <div class="home">
    <input type="text" v-model="searchText" />
    <table>
      <thead>
        <tr>
          <th>Имя</th>
          <th>Фамилия</th>
          <th>Возраст</th>
          <th>Действия</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="person of searchPersons" :key="person.id">
          <td>
            <span
              v-if="person.isEdit != 'firstName'"
              @click="onEdit(person.id, 'firstName')"
              v-html="person.firstName"
              class="item"
            ></span>
            <input
              type="text"
              v-else
              :value="person.firstName"
              @keypress.enter="onChange($event, person.id, 'firstName')"
            />
          </td>
          <td>
            <span
              v-if="person.isEdit != 'lastName'"
              @click="onEdit(person.id, 'lastName')"
              v-html="person.lastName"
              class="item"
            ></span>
            <input
              type="text"
              v-else
              v-model="person.lastName"
              @keypress.enter="onChange($event, person.id, 'lastName')"
            />
          </td>
          <td>
            <span
              v-if="person.isEdit != 'age'"
              @click="onEdit(person.id, 'age')"
              v-html="person.age"
              class="item"
            >
            </span>
            <input
              type="number"
              v-else
              v-model="person.age"
              @keypress.enter="onChange($event, person.id, 'age')"
            />
          </td>
          <td>
            <button class="btn" @click="deletePerson(person.id)">
              Удалить
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      searchText: null,
      persons: [
        {
          id: 1,
          firstName: "user1",
          lastName: "lastUser1",
          age: 25,
          isEdit: false,
        },
        {
          id: 2,
          firstName: "user1",
          lastName: "lastUser1",
          age: 25,
          isEdit: false,
        },
        {
          id: 3,
          firstName: "user1",
          lastName: "lastUser1",
          age: 25,
          isEdit: false,
        },
        {
          id: 4,
          firstName: "user1",
          lastName: "lastUser1",
          age: 25,
          isEdit: false,
        },
        {
          id: 5,
          firstName: "user1",
          lastName: "lastUser1",
          age: 25,
          isEdit: false,
        },
      ],
    };
  },
  methods: {
    onEdit(id, key) {
      const index = this.persons.findIndex((i) => i.id == id);
      this.persons.forEach((i) => {
        i.isEdit = false;
      });
      this.persons[index].isEdit = key;
    },
    onChange(e, id, name) {
      const index = this.persons.findIndex((i) => i.id == id);
      this.persons[index][name] = e.target.value;
      this.persons[index].isEdit = false;
    },
    deletePerson(id) {
      this.persons = this.persons.filter((i) => i.id != id);
    },
    onSearch() {},
  },
  computed: {
    searchPersons() {
      let results = [];
      const persons = [...this.persons];
      if (this.searchText) {
        persons.forEach((item) => {
          let isRes = false;
          Object.keys(item).forEach((key) => {
            if (key != "id") {
              if (item[key].toString().includes(this.searchText)) {
                item[key] = item[key].replace(
                  this.searchText,
                  `<span>${this.searchText}</span>`
                );
                isRes = true;
              }
            }
          });
          isRes && results.push(item);
        });
        return results;
      } else {
        return this.persons;
      }
    },
  },
};
</script>


