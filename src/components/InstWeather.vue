<template>
    <div class="inst">
        <h3>Real Feel</h3>
        <table v-for="item in this.instData" :key="item.id">
            <tr><td><span class="label">Temp:</span> {{ item.imperial.temp }} F ({{ item.imperial.heatIndex }} F)</td></tr>
            <tr><td><span class="label">Humidity:</span> {{ item.humidity }} % ({{ item.imperial.dewpt }}F)</td></tr>
            <tr><td><span class="label">Pressure:</span> {{ item.imperial.pressure }} mmHg </td></tr>
        </table>
    </div>
</template>

<script>


export default {
    name: 'InstWeather',
    props: {
    },
    data() {
        return {
            instData: []
        }
    },
    async mounted() {
        const response = await fetch('http://192.168.1.5:3031/forecast/instant');
        const jsondata = await response.json();
        this.instData = jsondata.observations;
        console.log(this.instData);
    },
}
</script>

<style scoped>
table,
tr,
th,
td {
    border-color: black;
    border: 1px solid;
    border-collapse:collapse;
    padding: 10px;
}

table{
    margin:auto;
}

th {
    font-size:14pt;
}

td{
    margin:5;
}
.label{
    font-weight:bold;
}
</style>