<template>
  <div class="container">
    <CardInline :firstName="name" />
    <app-aler :alert="alert" @close="alert = null">
    </app-aler>
    <form class="card" @submit.prevent="createPerson">
      <h2>Работа с базой данных</h2>

      <div class="form-control">
        <label for="name">введите имя</label>
        <input type="text" id="name" v-model.trim="name">
      </div>
      <button class="btn primary" :disabled="name.length === 0">создать человека</button>
    </form>

    <app-people-list :people="people" @load="loadPeople" @remove="removePerson"></app-people-list>
  </div>
</template>

<script>
import CardInline from './CardInline.vue'
import appAler from './appAler.vue'
import axios from 'axios'
import appPeopleList from './appPeopleList.vue'
import appAlerVue from './appAler.vue'

export default {
  data() {
    return {
      name: '',
      people: [],
      alert: null
    }
  },
  mounted() {
    this.loadPeople()
  },
  methods: {
    async createPerson() {
      const response = await fetch('https://vue-with-htpp-c2499-default-rtdb.firebaseio.com/people.json', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          firstName: this.name
        })
      })

      const data = await response.json()
      this.people.push({
        id: data.name,
        firstName: this.name,
      })

      this.name = ''
    },

    async loadPeople() {
      try {
        const { data } = await axios.get('https://vue-with-htpp-c2499-default-rtdb.firebaseio.com/people.json')

        if (!data) {
          throw new Error('список людей пуст')
        }

        this.people = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
      } catch (error) {
        this.alert = {
          type: 'danger',
          title: 'ОШИБКА',
          text: error.message
        }
        console.log(error.message)
      }

    },
    async removePerson(id) {
      await axios.delete(`https://vue-with-htpp-c2499-default-rtdb.firebaseio.com/people/${id}.json`)
      this.people = this.people.filter(person => person.id !== id)
    }
  },
  components: { appPeopleList, appAler, CardInline }
}
</script>

<style></style>
