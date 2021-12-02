<template>
  <div>
    <h1>Veeva Subject Line Calculator</h1>
    <div class="subjects">
      <div class="row" v-bind:key= "subject.id" v-for="subject in subjects">
        <input v-model="subject.msg">
        <button class="remove" @click="removeSubject(subject.id)">-</button>
      </div>
      <button class="add" @click="addSubject">+</button>
      <p :class="{ error: characters > 255}"><strong>Total Character Count:</strong> {{characters}} / 255</p>
      <p><i>Includes special formatting characters not shown.</i></p>
      <hr>
      <p><strong>Common Veeva Tokens for use in subjects:</strong></p>
      <ul>
        <li><i>##accTitle##</i> - Inserts Account salutation</li>
        <li><i>##accFname##</i> - Inserts Account first name</li>
        <li><i>##accLname##</i> - Inserts Account last name</li>
        <li><i>##userName##</i> - Inserts the rep’s name</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      charsBase: 16,
      n: 4,
      subjects: [
        { id: 1, msg: 'Subject 1' },
        { id: 2, msg: 'Subject 2' },
        { id: 3, msg: 'Subject 3' }
      ]
    }
  },
  computed: {
    characters () {
      let charCount = this.subjects.reduce(function(charCount, item) {
        return charCount + item.msg.length;
      }, 0)
      return this.charsBase + this.subjects.length - 1 + charCount
    }
  },
  methods: {
    addSubject () {
      this.n = this.n + 1;
      this.subjects.push({ id: this.n, msg: 'New Subject' })
    },
    removeSubject (id) {
      for (let i = 0; i < this.subjects.length; ++i) {
        if (this.subjects[i].id === id) {
          this.subjects.splice(i, 1)
          break
        }
      }
    }
  }
}
</script>

<style>
body {
  max-width: 650px;
  display: flex;
  flex-direction: column;
  margin: 20px auto;
  padding: 15px;
  font-size: 18px;
  line-height: 20px;
  font-family: arial, sans-serif;
}

input {
  font-size: 18px;
  line-height: 20px;
  font-family: arial, sans-serif;
  width: calc(100% - 50px);
}

.row {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 15px;
}

.remove {
  width: 29px;
}

.add {
  width: 50px;
}

.error {
  color: #E10000;
}
</style>
