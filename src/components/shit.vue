<template>
  <div class="flex flex-col gap-10 bg-slate-400 rounded-xl px-10 py-5">
    <p>Выберите файл в формате CSV:</p>
    <input id="file-reader" type="file" @change="showFile" />
    <div class="flex flex-row gap-5">
      <button @click="handlePlusClick" class="w-20 bg-green-200 rounded-md">
        +
      </button>
      <button @click="handleMinusClick" class="w-20 bg-red-100 rounded-md">
        -
      </button>
    </div>
    <div class="flex justify-center overflow-x-scroll" v-if="data">
      <table class="w-full border-separate border border-slate-400">
        <thead>
          <tr>
            <th
              class="border border-slate-300"
              v-for="element in headers.slice(0, limitOfElementsInRow)"
              :key="element"
            >
              {{ element }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="bg-slate-300"
            v-for="dataRow in data.slice(
              startPositionColumn,
              limitOfElementsInColumn
            )"
            :key="dataRow"
          >
            <td
              v-for="element in dataRow.slice(0, limitOfElementsInRow)"
              :key="element"
            >
              {{ element }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import Papa from "papaparse";

import { ref } from "vue";

const file = ref(null);

const data = ref(null);

const headers = ref(null);

const limitOfElementsInRow = 5;

const startPositionColumn = ref(0);

const limitOfElementsInColumn = ref(20);

const showFile = (e) => {
  let file = e.target.files[0];
  if (file.name.split(".").pop() != "csv") {
    alert("Неверный файл");
    document.getElementById("file-reader").value = null;
  } else {
    //alert(`File name: ${file.name} \nFile size: ${file.size} bytes`);
    Papa.parse(file, {
      complete: function (results) {
        console.log(results);
        headers.value = results.data[0];
        data.value = results.data.slice(1);
      },
    });
  }
};

const handlePlusClick = () => {
  startPositionColumn.value++;
  limitOfElementsInColumn.value++;
};
</script>

<style lang="scss" scoped></style>
