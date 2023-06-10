<template>
  <div>
    <main>
      <!--Modal-->
      <div class="overlay" v-if="isShowModal">
        <form class="modal" @submit.prevent="addNote">
          <button class="close top-right" @click="showModal(false)">X</button>
          <label class="note-label">What's on your mind ?</label>
          <textarea name="note" id="note" cols="30" rows="10" v-model.trim="newNote"></textarea>
          <p class="note-error-label" v-if="errorMsg">{{ errorMsg }}</p>
          <button type="submit">Add Note</button>
        </form>
      </div>
      <!--App-->
      <div class="container">
        <header>
          <h1>Notes</h1>
          <button @click="showModal(true)">+</button>
        </header>
        <div class="card-container">
          <p v-if="notes.length == 0">Hey, do you need note something ?</p>
          <div
            v-for="note in notes"
            class="card"
            :key="note.id"
            :style="{ backgroundColor: note.backgroundColor }"
          >
            <p class="main-text">
              {{ note.text }}
            </p>
            <p class="date">{{ note.date }}</p>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
//state
const isShowModal = ref(false)
const newNote = ref('')
const notes = ref([])
const requiredMsg = 'Note is required.'
const minLengthMsg = 'Note must be 10 characters or more.'
const errorMsg = ref('')
//methods
//show & close modal handler
function showModal(value) {
  isShowModal.value = value
  if (value == false) {
    clearNewNoteAndErrorMsgState()
  }
}
//add a new note
function addNote() {
  if (newNote.value.length == 0) {
    errorMsg.value = requiredMsg
    return
  }
  if (newNote.value.length > 0 && newNote.value.length < 10) {
    errorMsg.value = minLengthMsg
    return
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date().toLocaleString(),
    backgroundColor: getRandomColor()
  })
  //close modal
  showModal(false)
}
//get a random color
function getRandomColor() {
  const color = 'hsl(' + Math.random() * 360 + ', 100%, 75%)'
  return color
}
//watch
watch(newNote, (newValue, oldValue) => {
  if (newValue.length == 0) {
    errorMsg.value = requiredMsg
  } else if (newValue.length > 0 && newValue.length < 10) {
    errorMsg.value = minLengthMsg
  } else {
    errorMsg.value = ''
  }
})

//clear state
function clearNewNoteAndErrorMsgState() {
  newNote.value = ''
  errorMsg.value = ''
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

.note-label {
  font-size: 16px;
  font-weight: bold;
  color: #000000;
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

.card-container {
  display: flex;
  flex-wrap: wrap;
}
.card {
  width: 225px;
  height: 225px;
  background-color: yellowgreen;
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 0px 20px 20px 0px;
}

.date {
  font-size: 12px;
  font-weight: bold;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  width: 50vw;
  background-color: #ffffff;
  border-radius: 10px;
  padding: 20px;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}
.modal button {
  padding: 10px;
  margin-top: 15px;
  border: none;
  border-radius: 5px;
  color: #ffffff;
  background-color: #000000;
  cursor: pointer;
}
.modal .close {
  width: 30px;
  height: 30px;
  border-radius: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.top-right {
  position: absolute;
  top: -30px;
  right: -15px;
}

.note-error-label {
  color: red;
}
</style>