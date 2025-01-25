<script setup lang="ts">
    import { ref, onMounted } from 'vue';
    import type { Ref } from 'vue';

    type Geolocation = {
        latitude: number;
        longitude: number;
    };

    const coords: Ref<Geolocation | undefined> = ref();
    const geolocationBlockByuser: Ref<boolean> = ref(false);

    const getGeolocation = async(): Promise<void> => {
        navigator.geolocation.getCurrentPosition(
                async (position: {coords: Geolocation}) => {
                    coords.value = position.coords;
                },
            (error: {message: string}) => {
                geolocationBlockByuser.value = true;
                console.error(error.message);
            }        
        );
    };
    onMounted(async() => {
        getGeolocation();
    });

</script>

<template>
    <div v-if="coords && !geolocationBlockByuser">
        {{ coords.latitude }}
        {{ coords.longitude }}
    </div>
        
    <div v-else-if="coords && !geolocationBlockByuser">
        <p>geolocation is blocked by user</p>
    </div>
</template>

<style scoped>

</style>
