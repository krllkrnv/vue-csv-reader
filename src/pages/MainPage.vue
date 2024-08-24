<template>
  <div class="flex flex-col gap-5 bg-slate-400 rounded-xl px-10 py-5">
    <p>Выберите файл в формате CSV:</p>
    <input id="file-reader" type="file" @change="showFile" />
    <div
      class="flex flex-col gap-5 justify-center overflow-x-scroll"
      v-if="data"
    >
      <div class="flex flex-row gap-5 self-end">
        <div class="flex flex-row gap-2">
          <span>Строк на странице: </span>
          <select v-model="rowsPerPage" name="rowsPerPage" id="rowsPerPage">
            <option selected value="5">5</option>
            <option value="10">10</option>
            <option value="20">20</option>
            <option value="30">30</option>
          </select>
        </div>
        <span
          >{{
            Math.ceil(
              (startPositionRow + Number(rowsPerPage) / Number(rowsPerPage)) /
                Number(rowsPerPage)
            )
          }}
          из {{ Math.ceil(data.length / Number(rowsPerPage)) }}</span
        >
        <div class="flex flex-row gap-5">
          <button @click="handleMinusClick">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="size-5"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M15.75 19.5 8.25 12l7.5-7.5"
              />
            </svg>
          </button>
          <button @click="handlePlusClick">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="size-5"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="m8.25 4.5 7.5 7.5-7.5 7.5"
              />
            </svg>
          </button>
        </div>
      </div>
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
              startPositionRow,
              startPositionRow + Number(rowsPerPage)
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

const rowsPerPage = ref(5);

const limitOfElementsInRow = 5;

const startPositionRow = ref(0);

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
  startPositionRow.value = startPositionRow.value + Number(rowsPerPage.value);
};

const handleMinusClick = () => {
  if (startPositionRow.value - Number(rowsPerPage.value) > 0) {
    startPositionRow.value = startPositionRow.value - Number(rowsPerPage.value);
  } else {
    startPositionRow.value = 0;
  }
};
</script>

<style lang="scss" scoped></style>
