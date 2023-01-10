<template>
	<q-page class="bg-grey-3 column">
       <div class="row q-pa-sm bg-primary">
			<q-input
				filled
				bottom-slots
				bg-color="white"
                class="col"
                square
				dense
                placeholder="Search By City" 
                v-model="cityName"
                @keyup.enter="onSearch"
			>
				<template v-slot:append>
					<q-btn round dense flat icon="add" @click="onSearch" />
				</template>
			</q-input>
		</div>
        <ResultWeather v-if="weather !== null"  :weather="weather" :dailyWeather="dailyWeather" />
    </q-page>
</template>

<script>
import { ref,watch } from 'vue';
import ResultWeather from '@/components/ResultWeather.vue';
export default {
	name: 'IntegrationView',
    components : {
        ResultWeather
    },
	setup() {
        const cityName = ref('');
        const weather = ref(null);
        const dailyWeather = ref([]);

        const onSearch = async () => {
            let publicAPIKey = "e7983195ff4f09f0990a71df010c970a"
            // &units=imperial&appid
            const req = await fetch(
                `https://api.openweathermap.org/data/2.5/weather?q=${cityName.value}&units=imperial&exclude=minutely,hourly&appid=${publicAPIKey}`,
                { method: "GET" }
            );
            const result = await req.json();
            if (result.cod !== "400") {
                let data = {
                    latitude : result.coord.lat,
                    longtitude : result.coord.lon
                }
                weather.value = data;

                const newRequest = await fetch(
                    `https://api.openweathermap.org/data/2.5/onecall?lat=${data.latitude}&lon=${data.longtitude}&units=imperial&exclude=minutely,hourly&appid=${publicAPIKey}`,
                    { method: "GET" }
                );
                const newResult = await newRequest.json();
                dailyWeather.value = newResult.daily

            }
        }
       
        return {cityName,onSearch,weather,dailyWeather}

	},
};
</script>

