<template>
    <Suspense>
    <div class="weather">
        <InstWeather />
        <table>
            <tr>
                <td>
                    <DailyWeather />
                </td>
                <td>
                    <HourlyWeather />
                </td>
            </tr>
        </table>
        </div>
    </Suspense>
</template>
  
<script>

import HourlyWeather from '@/components/HourlyWeather.vue';
import DailyWeather from '@/components/DailyWeather.vue';
import InstWeather from '@/components/InstWeather.vue';

export default {
    name: 'WeatherView',
    components: {
        HourlyWeather, DailyWeather, InstWeather
    }
}
</script>


<style scoped>
td{
    vertical-align:top;
    padding:0px 100px;
}


</style>