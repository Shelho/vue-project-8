<template>
  <div v-if="people.length !== 0">
    <CardInline v-for="person in people " :key='person.id' :firstName="person.firstName"
      @remove="$emit('remove', person.id)" />
  </div>
  <div class="card center" v-else>
    <h4>список пока пустой</h4>
    <button class="btn" @click="$emit('load')">загрузить список</button>
  </div>
</template>

<script>
import CardInline from './CardInline.vue'
export default {
  emits: ['load', 'remove'],
  props: ['people'],
  components: { CardInline }
}
</script>