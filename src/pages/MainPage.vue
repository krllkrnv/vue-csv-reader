<template>
  <div class="flex flex-col gap-5 bg-slate-400 rounded-xl px-10 py-5">
    <p>Выберите файл в формате CSV:</p>
    <input id="file-reader" type="file" @change="showFile" />
    <div
      class="flex flex-col gap-5 justify-center overflow-x-scroll"
      v-if="state.data"
    >
      <div class="flex flex-row gap-5 self-end">
        <div class="flex flex-row gap-2">
          <span>Строк на странице: </span>
          <select
            v-model="state.rowsPerPage"
            name="rowsPerPage"
            id="rowsPerPage"
          >
            <option selected value="5">5</option>
            <option value="10">10</option>
            <option value="20">20</option>
            <option value="30">30</option>
          </select>
        </div>
        <span
          >{{
            Math.ceil(
              (state.startPositionRow +
                Number(state.rowsPerPage) / Number(state.rowsPerPage)) /
                Number(state.rowsPerPage)
            )
          }}
          из
          {{ Math.ceil(state.data.length / Number(state.rowsPerPage)) }}</span
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
              v-for="element in state.headers.slice(
                0,
                state.limitOfElementsInRow
              )"
              :key="element"
            >
              {{ element }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="bg-slate-300"
            v-for="dataRow in state.data.slice(
              state.startPositionRow,
              state.startPositionRow + Number(state.rowsPerPage)
            )"
            :key="dataRow"
          >
            <td
              v-for="element in dataRow.slice(0, state.limitOfElementsInRow)"
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
import { reactive } from "vue";

const state = reactive({
  file: null,
  data: null,
  headers: null,
  rowsPerPage: 5,
  limitOfElementsInRow: 5,
  startPositionRow: 0,
});

const showFile = (e) => {
  let file = e.target.files[0];
  if (file.name.split(".").pop() !== "csv") {
    alert("Неверный файл");
    document.getElementById("file-reader").value = null;
  } else {
    Papa.parse(file, {
      complete: function (results) {
        console.log(results);
        state.headers = results.data[0];
        state.data = results.data.slice(1);
      },
    });
  }
};

const handlePlusClick = () => {
  state.startPositionRow += Number(state.rowsPerPage);
};

const handleMinusClick = () => {
  if (state.startPositionRow - Number(state.rowsPerPage) > 0) {
    state.startPositionRow -= Number(state.rowsPerPage);
  } else {
    state.startPositionRow = 0;
  }
};
</script>

<style lang="scss" scoped></style>
