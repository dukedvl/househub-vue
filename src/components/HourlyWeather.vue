<template>
    <div class="hourly">
        <h3>Hourly</h3>
        <table>
            <th>Time</th>
            <th>Temp</th>
            <th>Feels Like</th>
            <th>Weather</th>
            <th>Humidity</th>
            <th>Dew Point</th>
            <tr v-for="item in hourlydata" :key="item.weather_time">
                <td>{{ new Date(item.weather_time).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }) }}</td>
                <td>{{ item.temp }} F</td>
                <td>{{ item.feels_like }} F</td>
                <td><img :src=getWeatherImg(item.weather_code,item.weather_time) height="24" width="24" />{{
    this.getDisplayString(item.weather_code) }}</td>
                <td>{{ item.humidity }}%</td>
                <td>{{ item.dew_point }} F</td>
            </tr>
        </table>
    </div>
</template>

<script>
import { DateTime } from "luxon";


export default {
    name: 'HourlyWeather',
    props: {
    },
    data() {
        return {
            hourlydata: []
        }
    },
    async mounted() {
        const response = await fetch('http://192.168.1.5:3031/forecast/hourly');
        this.hourlydata = await response.json();
        console.log(response);
    },
    setup() {
        function getDisplayString(string) {
            return string.replace("_", " ");
        }

        function getWeatherImg(conditions, timeOfDay) {

            //return "assets/001-sunny.png";
            let dayTime = true;

            if (timeOfDay != null && this.today != null) {
                let hour = DateTime.fromISO(timeOfDay).hour;
                dayTime = hour > DateTime.fromISO(this.today.sunrise_time).hour && hour < DateTime.fromISO(this.today.sunset_time).hour;
            }

            conditions = conditions.toLowerCase();

            if (conditions.includes("partly") || conditions.includes("mostly")) {
                return dayTime ? require("../assets/011-sunny.png") : require("../assets/013-full moon.png");
            }
            else if (conditions.includes("cloud")) {
                return dayTime ? require("../assets/002-cloud.png") : require("../assets/013-full moon.png");
            }

            if (conditions.includes("rain") || conditions.includes("drizzle")) {
                return require("../assets/004-rain.png");
            }

            if (conditions.includes("fog")) {
                return dayTime ? require("../assets/019-fog.png") : require("../assets/029-full moon.png");
            }

            if (conditions.includes("snow") || conditions.includes("flurries")) {
                return require("../assets/007-snow.png");
            }

            if (conditions.includes("wind")) {
                return require("../assets/012-windy.png");
            }

            if (conditions.includes("hail")) {
                return require("../assets/014-hail.png");
            }

            if (conditions.includes("sleet")) {
                return require("../assets/027-sleet.png");
            }

            if (conditions.includes("storm")) {
                return require("../assets/006-thunder.png");
            }

            return dayTime ? require("../assets/001-sunny.png") : require("../assets/008-full moon.png");

        }

        return { getDisplayString, getWeatherImg };
    }
}
</script>

<style scoped>
table,
tr,
th,
td {
    border-color: black;
    border: 1px solid;
    border-collapse: collapse;
    padding: 10px;
}

th {
    font-size: 14pt;
}

td {
    margin: 5;
}

img{
    float: left;
    margin: 0px 15px 0px 5px;
}
</style>