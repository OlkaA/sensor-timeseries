<template>
    <div class="reports-table">
        <h3>Aggregated information about all the batches.</h3>
        <div class="table-wrapper">
            <table>
                <thead>
                    <tr>
                        <th rowspan="2">Batch\Sensor</th>
                        <th v-for="header in tableData.headers" :key="header" colspan="5"> {{ header }}</th>
                    </tr>
                    <tr>
                        <th v-for="header in tableData.headers" :key="header" colspan="5">
                            <div class="cell-space">
                                <div v-for="value in columnValues" :key="value">
                                    {{ value }}
                                </div>
                            </div>
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="batch in tableData.body" :key="batch.name">
                        <td class="batch-info">
                            <div class="name">{{ batch.name }}</div>
                            <div class="time"><span>Start:</span>{{ batch.startTime }}</div>
                            <div class="time"><span>End:</span>{{ batch.endTime }}</div>
                            <div class="time"><span>Duration:</span>{{ batch.duration }}</div>
                        </td>
                        <td v-for="info in batch.row" :key="info.name" colspan="5">
                            <div class="cell-space">
                                <div>{{ info.start }}</div>
                                <div>{{ info.end }}</div>
                                <div>{{ info.min }}</div>
                                <div>{{ info.avr }}</div>
                                <div>{{ info.max }}</div>
                            </div>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
  
<script>
export default {
    name: 'ReportsTable',
    props: {
        sensorData: Object
    },
    data() {
        return {
            columnValues: ['start', 'end', 'min', 'avr', 'max']
        }
    },
    computed: {
        tableData() {
            let data = {
                headers: [],
                body: []
            };
            for (const batch in this.sensorData) {
                let tableBatch = {
                    name: batch,
                    row: []
                }
                for (const sensor in this.sensorData[batch]) {
                    const header = `${sensor} (${this.sensorData[batch][sensor].unit})`;

                    if (!data.headers.includes(header)) {
                        data.headers.push(header);
                    }
                    // Form first column
                    if (!tableBatch['startTime']) {
                        tableBatch['startTime'] = this.sensorData[batch][sensor].timestamps[0];
                        tableBatch['endTime'] = this.sensorData[batch][sensor].timestamps[this.sensorData[batch][sensor].timestamps.length - 1];
                        tableBatch['duration'] = this.getTime(new Date(tableBatch['endTime']) - new Date(tableBatch['startTime']));
                    }
                    // Form data for other columns
                    tableBatch.row.push({
                        name: sensor,
                        min: Math.min(...this.sensorData[batch][sensor].values).toFixed(2),
                        max: Math.max(...this.sensorData[batch][sensor].values).toFixed(2),
                        avr: (this.sensorData[batch][sensor].values.reduce((a, b) => a + b, 0) / this.sensorData[batch][sensor].values.length).toFixed(2),
                        start: this.sensorData[batch][sensor].values[0].toFixed(2),
                        end: this.sensorData[batch][sensor].values[this.sensorData[batch][sensor].values.length - 1].toFixed(2)
                    })
                }

                data.body.push(tableBatch);
            }
            return data;
        }
    },
    methods: {
        getTime(ms) {
            const minutes = ms / 1000 / 60;
            return `${minutes.toFixed()}min. / ${(minutes / 60).toFixed(2)}h.`;
        }
    }
}
</script>
  
<style scoped lang="scss">
@import '../assets/scss/variables.scss';

.reports-table {
    padding: 2rem;

    h3 {
        font-family: "Calibri";
        margin-top: 0;
        font-weight: 400;
        text-align: center;
    }

    .table-wrapper {
        box-shadow: $shadow;
        overflow-x: auto;
        max-height: 70vh;
    }

    table {
        position: relative;
        font-family: Arial, Helvetica, sans-serif;
        border-collapse: collapse;
        width: 100%;
    }

    table td,
    table th {
        border: 1px solid $grey;
        height: 100%;

        .cell-space {
            display: flex;
            justify-content: space-around;
            border-right: white;

            div {
                flex: 1;
                text-align: center;
                padding-top: 12px;
                padding-bottom: 12px;
                min-width: 45px;
            }
        }
    }

    table tr:first-child th {
        padding-top: 12px;
        padding-bottom: 12px;
        text-align: center;
    }

    table tr:nth-child(even) {
        background-color: $light-grey;
    }

    table tr:first-child th:first-child,
    table tr td:first-child {
        width: 200px;
    }

    table tr {
        th {
            text-align: left;
            background-color: $green;
            color: white;
            position: sticky;
            top: -1px;
        }

        &:nth-child(2) {
            th {
                top: 43px;
            }
        }
    }

    table tr td {
        font-size: 12px;
    }

    .batch-info {
        padding: 12px;

        * {
            display: flex;
            flex-direction: column;

            &.name {
                font-size: 14px;
                letter-spacing: 1.4px;
            }

            &.time {
                font-size: 12px;
                letter-spacing: 1.2px;
                margin-top: 4px;
            }
        }
    }
}
</style>  