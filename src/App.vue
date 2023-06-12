<template>
  <div>
    <main>
      <!--Modal-->
      <CustomModal v-if="isShowModal" @closeModal="showModal(false)" @addNote="addNote"></CustomModal>
      <!--App-->
      <div class="container">
        <header>
          <h1 class="note">Notes</h1>
          <button @click="showModal(true)">+</button>
        </header>
        <div class="card-container">
          <p v-if="notes.length == 0">Hey, do you need note something ?</p>
          <div v-for="note in notes" class="card" :key="note.id" :style="{ backgroundColor: note.backgroundColor }">
            <div class="card-content">
              <p>{{ note.text }}</p>
              <button class="remove-card-btn" @click="removeNote(note.id)">X</button>
            </div>
            <p class="date">{{ note.date }}</p>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import CustomModal from './components/CustomModal.vue';

//state
const isShowModal = ref(false)
const notes = ref([])
//methods
//show & close modal handler
function showModal(value) {
  isShowModal.value = value
}
//add a new note
function addNote(note) {
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: note,
    date: new Date().toLocaleString(),
    backgroundColor: getRandomColor()
  })
  //close modal
  showModal(false)
}
//remove note
function removeNote(id) {
  const newNotes = notes.value.filter((obj) => obj.id !== id);
  notes.value = newNotes;
}
//get a random color
function getRandomColor() {
  const color = 'hsl(' + Math.random() * 360 + ', 100%, 75%)'
  return color
}
</script>

<style scoped>
main {
  width: 100vw;
  height: 100vh;
}

.container {
  max-width: 1024px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  color: #000000;
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  border-radius: 100%;
  cursor: pointer;
  color: #ffffff;
  background-color: #000000;
  font-size: 20px;
}



.remove-card-btn {
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 100%;
  cursor: pointer;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
}

.card {
  width: 225px;
  height: 225px;
  word-wrap: break-word;
  background-color: yellowgreen;
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 0px 20px 20px 0px;
}

.card-content {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  word-wrap: break-word;
}

.date {
  font-size: 12px;
  font-weight: bold;
}
</style>