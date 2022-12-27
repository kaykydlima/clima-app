<template>
    <v-card class="mx-auto" max-width="368">
        <v-card-item :title="props.obj.location.name">
            <template v-slot:subtitle>
                <div class="d-flex align-center">
                    <img :src="props.obj.current.condition.icon" class="image">
                    <div>
                        {{ props.obj.current.condition.text }}
                    </div>
                </div>
            </template>
        </v-card-item>

        <v-card-text class="py-0">
            <v-row align="center" no-gutters>
                <v-col class="text-h2" cols="6">
                    {{ Math.floor(props.obj.current.temp_c) }}&deg;C
                    <v-card-subtitle tag="h3" class="px-0">
                        Feelslike: {{ props.obj.current.feelslike_c }}&deg;C
                    </v-card-subtitle>    
                    <v-card-subtitle tag="h3" class="px-0">
                        {{ Math.floor(props.dayOne.day.maxtemp_c) }}&deg;C/{{ Math.floor(props.dayOne.day.mintemp_c) }}&deg;C
                    </v-card-subtitle>
                </v-col>

                <v-col cols="6" class="text-right">
                    <img :src="props.obj.current.condition.icon" class="image-big">
                </v-col>
            </v-row>
        </v-card-text>

        <div class="d-flex py-3 justify-space-between">
            <v-list-item density="compact" prepend-icon="mdi-weather-windy">
                <v-list-item-subtitle>{{ props.obj.current.gust_kph }} km/h</v-list-item-subtitle>
            </v-list-item>
            <v-list-item density="compact" prepend-icon="mdi-weather-pouring">                
                    <v-list-item-subtitle>{{ props.dayOne.day.daily_chance_of_rain }}%</v-list-item-subtitle>
            </v-list-item>
        </div>

        <v-expand-transition>
            <div v-if="state.expand">
                <v-list class="bg-transparent" v-for="item in props.day">
                    <div v-for="(moonPhase, index) in moonPhases">
                        <v-list-item title="Moon phase" v-if="moonPhase.type === item.astro.moon_phase" :prepend-icon="moonPhase.icon" :key="index">
                            <v-list-item-subtitle>
                                {{ moonPhase.type }}
                            </v-list-item-subtitle>
                        </v-list-item>
                    </div>
                    <div class="d-flex py-3 justify-space-between">
                        <v-list-item prepend-icon="mdi-weather-sunset-up">
                            <v-list-item-subtitle>
                                {{ item.astro.sunrise }}                                                
                            </v-list-item-subtitle>    
                        </v-list-item>
                        <v-list-item prepend-icon="mdi-weather-sunset-down">
                            <v-list-item-subtitle>
                                {{ item.astro.sunset }}
                            </v-list-item-subtitle>
                        </v-list-item>
                    </div>
                    <v-list-item class="text-center">
                        <v-list-item-subtitle>{{ props.obj.location.tz_id }} - {{ props.obj.location.country }}</v-list-item-subtitle>
                    </v-list-item>
                </v-list>
            </div>
        </v-expand-transition>

        <v-divider></v-divider>

        <v-card-actions>
            <v-btn @click="state.expand = !state.expand">
                {{ !state.expand ? 'Full Report' : 'Hide Report' }}
            </v-btn>
        </v-card-actions>
    </v-card>
</template>
<script setup>
    import { ref } from 'vue';
    const props = defineProps(['day', 'obj', 'dayOne'])

    const state = ref({
        expand: false,
    })

    const moonPhases = [
        {
            'type': 'New Moon',    
            'icon': 'mdi-moon-new'
        },
        {
            'type': 'Waxing Crescent',    
            'icon': 'mdi-moon-waxing-crescent'
        },
        {
            'type': 'First Quarter',    
            'icon': 'mdi-moon-first-quarter'
        },
        {
            'type': 'Waxing Gibbous',    
            'icon': 'mdi-moon-waxing-gibbous'
        },
        {
            'type': 'Full Moon',    
            'icon': 'mdi-moon-full'
        },
        {
            'type': 'Waning Gibbous',    
            'icon': 'mdi-moon-waning-gibbous'
        },
        {
            'type': 'Last Quarter',    
            'icon': 'mdi-moon-last-quarter'
        },
        {
            'type': 'Waning Crescent', 
            'icon': 'mdi-moon-waning-crescent'
        }
    ]

</script>
  
<style scoped>
    .image {
        width: 32px;
    }

    .image-big {
        width: 100px;
    }
</style>