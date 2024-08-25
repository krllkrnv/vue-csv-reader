# Vue 3 + Vite + Tailwind + Papaparse

## To run locally
cd vue-csv-reader
#
npm install
#
npm run dev

## To create docker-container
cd vue-csv-reader 
#
docker build -t vuejs-app .
#
docker run -it -p 8085:80 --rm --name vuejs-app-1 vuejs-app
