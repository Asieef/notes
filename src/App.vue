<script setup>
import { ref } from "vue";

const showModal = ref(false);

const openModal = () => {
  showModal.value = true;
};

const closeModal = () => {
  showModal.value = false;
};

const newNote = ref("");
const notes = ref([]);

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = "Notes need to 10 Chracter At Least!!");
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    bgColor: getRandomColor(),
  });
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
};

const errorMessage = ref("");
</script>

<template>
  <div>
    <div class="container mx-auto">
      <!-- Modal Section -->

      <div
        @click="closeModal"
        v-if="showModal"
        class="absolute h-full w-full z-10 bg-opacity-90 bg-black flex items-center justify-center"
      >
        <div class="flex justify-center">
          <div
            @click.stop
            class="mb-3 xl:w-96 bg-white rounded p-8 bg-opacity-100"
          >
            <div class="flex flex-col gap-1">
              <textarea
                v-model.trim="newNote"
                class="form-control block w-full h-32 px-3 py-1.5 text-base font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none"
                id="exampleFormControlTextarea1"
                rows="3"
                placeholder="Your message"
              ></textarea>

              <p v-if="errorMessage" class="text-red-600">{{ errorMessage }}</p>
              <button
                @click="addNote"
                class="bg-purple-700 text-white text-center w-full p-1 hover:bg-purple-600"
              >
                Add Note
              </button>

              <button
                @click="closeModal"
                class="bg-red-700 text-white text-center w-full p-1 hover:bg-red-600"
              >
                Close
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Header Section -->
      <div class="flex items-center justify-between p-8">
        <div>
          <h2 class="text-6xl font-extrabold">Notes</h2>
        </div>
        <div class="p-4">
          <button
            @click="openModal"
            class="font-extrabold bg-black rounded-full text-white w-12 h-12 hover:bg-gray-700"
          >
            +
          </button>
        </div>
      </div>

      <!-- Card Section -->

      <div class="grid grid-cols-4 gap-4 px-8">
        <div
          v-for="(note, index) in notes"
          :key="index"
          class="rounded h-48 p-4 col-span-1 flex flex-col justify-between"
          :style="{ backgroundColor: note.bgColor }"
        >
          <p>
            {{ note.text }}
          </p>
          <p>{{ note.date.toLocaleDateString("en-us") }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
