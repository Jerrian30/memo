<script setup>
import { ref } from 'vue';

const modalOpen = ref(false);
const newMemo = ref("");
const memos = ref([]);
const errorMessage=ref("");

const backgroundRandomColor = () => `#${Math.floor(Math.random() * 16777215).toString(16).padStart(6, '0')}`;
const textRandomColor = () => `#${Math.floor(Math.random() * 16777215).toString(16).padStart(6, '0')}`;



function addMemo(){
  if(!newMemo.value){
    errorMessage.value="Memo cannot be empty!";
    return;
  }
  memos.value.push({
    id: Date.now(),
    memo: newMemo.value,
    date:new Date().toLocaleDateString("en-GB"),
    backgroundColor:backgroundRandomColor(),
    textColor:textRandomColor()
  });
  newMemo.value="";
  modalOpen.value=false;
  errorMessage.value="";
}

</script>

<template>
  <main>
    <div class="container m-14">
      <header>
        <div class="flex flex-col gap-6">
          <div class="text-4xl font-bold">Memo</div>
          <button @click="modalOpen=true" class="bg-blue-400 h-9 w-9 rounded-full text-white text-lg">+</button>
          <div class="flex gap-3 flex-wrap">
            <div v-for="(memo, index) in memos" :key="index" class="relative" >
              <div @click="memos.splice(index,1)" class="h-2 absolute right-2 cursor-pointer text-xl font-bold">&times;</div>
              <div class="p-2 flex flex-col h-[200px] w-[200px] justify-between py-4" :style="{backgroundColor:memo.backgroundColor, color:memo.textColor}">
                <p>{{ memo.memo }}</p>
                <p>{{ memo.date }}</p>
              </div>
              
            </div>
          </div>
        </div>
      </header>

      <!-- Background Blur ketika modal terbuka -->
      <div v-if="modalOpen" class="fixed inset-0 bg-black bg-opacity-50 backdrop-blur-sm z-10" @click="closeModal"></div>

      <!-- Modal -->
      <div v-if="modalOpen" class="fixed inset-0 flex justify-center items-center z-10">
        <div class="bg-white rounded-lg shadow-lg max-w-lg w-full animate__animated animate__slideInUp relative p-10">
          <p class="text-red-500 italic">{{ errorMessage }}</p>
          <div class="flex justify-between ">
            <div class="invisible"></div>
            <div class="cursor-pointer text-3xl font-bold absolute right-7 top-2" @click="modalOpen=false">&times;</div>
          </div>
            <div class="mb-4 flex flex-col">
            <textarea v-model="newMemo" name="" id="" cols="30" rows="10" class=" border-2 border-gray-500 rounded-md mt-1"></textarea>
            <button @click="addMemo" class="bg-gray-800 text-white py-2.5 font-semibold text-2xl rounded-md mt-4 w-full">Save</button>
            </div>
        </div>
      </div>
    </div>
  </main>
</template>
