<template>
    <div class="overlay">
        <form class="modal" @submit.prevent="emitAddNote">
            <button class="close top-right" @click="$emit('closeModal')">X</button>
            <label class="note-label">What's on your mind ?</label>
            <textarea name="note" id="note" cols="30" rows="10" v-model.trim="newNote"></textarea>
            <p class="note-error-label" v-if="errorMsg">{{ errorMsg }}</p>
            <button type="submit">Add Note</button>
        </form>
    </div>
</template>

<script setup>
import { ref, watch } from 'vue'
const emit = defineEmits(['closeModal', 'addNote'])
//state
const newNote = ref('')
const requiredMsg = 'Note is required.'
const minLengthMsg = 'Note must be 10 characters or more.'
const errorMsg = ref('')

//methods
function emitAddNote() {
    if (newNote.value.length == 0) {
        errorMsg.value = requiredMsg
        return
    }
    if (newNote.value.length > 0 && newNote.value.length < 10) {
        errorMsg.value = minLengthMsg
        return
    }
    emit('addNote', newNote);
}

//watch
watch(newNote, (newValue) => {
    if (newValue.length == 0) {
        errorMsg.value = requiredMsg
    } else if (newValue.length > 0 && newValue.length < 10) {
        errorMsg.value = minLengthMsg
    } else {
        errorMsg.value = ''
    }
})
</script>

<style  scoped>
.note-label {
    font-size: 16px;
    font-weight: bold;
    color: #000000;
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