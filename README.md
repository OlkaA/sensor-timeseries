
## Sensors Timeseries

## 📝 Table of Content

- [About](#about)
- [Used technologies](#tech)
- [Getting started](#getting-started)


## About <a name = "about"></a>

This project was made to visualize sensor timeseries data from a cultivation tank in the active pharmaceutical ingredient (API) production. When a cell cultivation starts, it is given a unique BatchID.
The data consists of timeseries data for multiple batches. Each batch contains data from 4
sensors.

The navigation bar at the top can bring to two different views.
1st view visualizes the batch data for different sensors in the form of graphs. The user should be able to filter between batches and sensors to compare data.
2nd view shows aggregated information about all the batches in the table, where user can see valuable information about process in batches (e.g., minimum, maximum, average, initial, last values of sensors for each batch) and can compare it.

## Used technologies <a name = "tech"></a>
- [Vue.js](https://vuejs.org/)
- [Modern & Interactive Open-source Charts](https://apexcharts.com/)

## Getting started <a name = "getting-started"></a>
- clone repo: 'https://github.com/OlkaA/sensor-timeseries.git'
- run 'npm install'
- run 'npm run serve'
- enjoy the project