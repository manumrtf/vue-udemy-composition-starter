<template>
  <h2>Create a new Participant</h2>
  <p>Total capacity: {{capacity}}</p>
  <form @submit.prevent="addNewParticipant()">
    <label for="Name">Name</label>
    <input v-model="name" id="name" />
    <label for="Age">Age</label>
    <input v-model="age" id="age" type="number" />
    <br />
    <input type="submit" />

  </form>

  <div>
    <div v-if="creators">
      <h2>
        Creators
      </h2>

      <ul>

        <li v-for="creator of creators" :key="creator.id">
          {{creator.username}}
        </li>
      </ul>
    </div>
    <h2>Participants</h2>
    <button @click="removeNonAdultsParticipants()">Keep only adults</button>
    <ul>

      <li v-for="(participant, index) of participants" :key="index">
        Name: {{participant.name}},
        age: {{participant.age}}
      </li>
    </ul>

  </div>
</template>

<script>


export default {
  name: 'EventDetails',
  props: {
    msg: String
  },
  data() {
    return {
      name: 'Pepe',
      age: 10,
      participants: [],
      capacity: 10,
      creators: null
    }
  },
  methods: {
    addNewParticipant() {
      if (this.spaceLeft === 0) {
        return alert("Reached maximum participants")
      }
      this.participants.push({ name: this.name, age: this.age })
    },
    removeNonAdultsParticipants() {
      this.participants = this.participants.filter(participant => participant.age >= 18)
    }
  },

  computed: {

    spaceLeft() {
      return this.capacity - this.participants.length
    }

  },

  async created() {
    const response = await fetch("https://jsonplaceholder.typicode.com/users")
    const result = await response.json()
    this.creators = result
  }
}
</script>

<style scoped>
label {
  display: block;

}

input {
  margin: 1rem 0;
}

ul {
  list-style: none;
}

li {
  margin: 1rem 0;
}
</style>
