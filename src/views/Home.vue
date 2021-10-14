<template>
  <div class="home">
    <div class="home__nav">
      <input type="text" v-model="searchText" placeholder="Поиск" />
      <select v-model="city">
        <option :value="null" disabled>Выберите город</option>
        <option :value="item.name" v-for="(item, i) of cities" :key="i">
          {{ item.name }}
        </option>
      </select>
      <button class="btn" v-if="city" @click="clearFilter">Очистить</button>
    </div>
    <table>
      <thead>
        <tr>
          <th>Имя</th>
          <th>Фамилия</th>
          <th>Возраст</th>
          <th>Город</th>
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
              type="text"
              v-else
              v-model="person.age"
              @keypress.enter="onChange($event, person.id, 'age')"
            />
          </td>
          <td>
            <span
              v-if="person.isEdit != 'city'"
              @click="onEdit(person.id, 'city')"
              v-html="person.city"
              class="item"
            >
            </span>
            <input
              type="text"
              v-else
              v-model="person.city"
              @keypress.enter="onChange($event, person.id, 'city')"
            />
          </td>
          <td>
            <button class="btn btn-danger" @click="deletePerson(person.id)">
              Удалить
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <pre></pre>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      searchText: null,
      city: null,
      cities: [
        { name: "Москва" },
        { name: "Питер" },
        { name: "Ташкент" },
        { name: "Мадрид" },
        { name: "Оттава" },
      ],
      persons: [
        {
          id: 1,
          firstName: "user1",
          lastName: "lastUser1",
          age: 25,
          isEdit: false,
          city: "Москва",
        },
        {
          id: 2,
          firstName: "user1",
          lastName: "lastUser1",
          age: 25,
          isEdit: false,
          city: "Питер",
        },
        {
          id: 3,
          firstName: "user1",
          lastName: "lastUser1",
          age: 25,
          isEdit: false,
          city: "Ташкент",
        },
        {
          id: 4,
          firstName: "user1",
          lastName: "lastUser1",
          age: 25,
          isEdit: false,
          city: "Мадрид",
        },
        {
          id: 5,
          firstName: "user1",
          lastName: "lastUser1",
          age: 25,
          isEdit: false,
          city: "Оттава",
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
    clearFilter() {
      this.city = null;
      this.searchText = null;
    },
  },
  computed: {
    searchPersons() {
      let results = [];
      if (this.searchText) {
        this.persons.forEach((item) => {
          const obj = { ...item };
          console.log(obj == item);
          Object.keys(obj).forEach((key) => {
            if (key != "id" && key != "city") {
              if (
                obj[key]
                  .toString()
                  .toLowerCase()
                  .includes(this.searchText.toLowerCase())
              ) {
                obj[key] = obj[key]
                  .toString()
                  .toLowerCase()
                  .replace(
                    this.searchText.toLowerCase(),
                    `<span>${this.searchText}</span>`
                  );
              }
            }
          });
          results.push({ ...obj });
        });
      } else {
        results = [...this.persons];
      }
      if (this.city) {
        console.log(this.city);
        results = results.map((i) => {
          const obj = { ...i };
          obj.city =
            obj.city == this.city ? `<span>${obj.city}</span>` : obj.city;
          return obj;
        });
      }

      return results;
    },
  },
};
</script>


