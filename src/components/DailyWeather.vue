<template>
    <div class="daily">
        <h3>Daily</h3>
        <table>
            <th>Day</th>
            <th>High</th>
            <th>Low</th>
            <th>Weather</th>
            <th>Moon</th>
            <tr v-for="item in dailydata" :key="item.weather_time">
                <td>{{ new Date(item.weather_time).toLocaleDateString([], {
                    month: 'short', day: '2-digit', weekday: 'short'
                }) }}</td>
                <td>{{ item.high }} F</td>
                <td>{{ item.low }} F</td>
                <td>{{ this.getDisplayString(item.weather_code) }}</td>
                <td><img :src=getWeatherImg(item.weather_code) height="24" width="24">{{ this.getDisplayString(item.weather_code) }}</td>
                <td><img :src=this.getMoonImg(item.moon_phase) height="24" width="24" />{{ this.getDisplayString(item.moon_phase) }}</td>
            </tr>
        </table>
    </div>
</template>

<script>
import { DateTime} from "luxon";

export default {
    name: 'DailyWeather',
    props: {
    },
    data() {
        return {
            today:{},
            dailydata: [],
        }
    },
    async mounted() {
        const response = await fetch('http://192.168.1.5:3031/forecast/daily');
        this.dailydata = await response.json();
        this.today=(this.dailydata.filter(day => DateTime.fromISO(day.weather_time).hasSame(DateTime.now(), 'day'))[0]);

        console.log(response);
    },

    setup() {
        function getDisplayString(string) {
            return string.replace("_", " ");
        }

        function getMoonImg(phase) {
            switch (phase) {
                case "New":
                    return require("../assets/new-moon-phase-circle.png");
                case "Waxing_Crescent":
                    return require("../assets/moon-phase-interface-symbol.png");
                case "First_Quarter":
                    return require("../assets/half-moon-phase-symbol.png");
                case "Waxing_Gibbous":
                    return require("../assets/moon-phase-symbol-9.png");
                case "Full":
                    return require("../assets/moon-phase.png");
                case "Waning_Gibbous":
                    return require("../assets/moon-phase-symbol-14.png");
                case "Third_Quarter":
                    return require("../assets/moon-phase-symbol-3.png");
                case "Waning_Crescent":
                    return require("../assets/moon-phase-symbol-12.png");
                default:
                    return require("../assets/new-moon-phase-circle.png");
            }

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

        return { getDisplayString, getMoonImg, getWeatherImg };
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