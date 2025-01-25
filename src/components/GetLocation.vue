<script setup lang="ts">
    import {ref, onMounted} from 'vue'
    import type {Ref} from 'vue'

    type Geolocation ={
        latitude: number;
        longitude: number;
    };

    const coords: Ref<Geolocation | undefined> = ref();    
    const geolocationBlockUser: Ref<boolean> = ref(false);

/*
    const getGeolocation = async(): Promise<void> =>{

        await navigator.geolocation.getCurrentPosition(
            () => {},
            (error: {message:string}) => {
                geolocationBlockUser.value = true;
                console.error(error.message);
            }


        );


    };*/


 const getGeolocation = (): void => {
         navigator.geolocation.getCurrentPosition(          
                async(position: {coords: Geolocation}) => {
                    coords.value = position.coords
                },
                (error: {message: string}) => {
                    geolocationBlockUser.value = true;
                    console.error(error.message);
            }       
        

        );


    };

    onMounted(async() => {
        getGeolocation();
    });



</script>

<template>

    <div v-if="coords && !geolocationBlockUser">
        <p>Latitude: {{coords.latitude}}</p>
        <p>Longitude: {{coords.longitude}}</p>
    </div>

    <div v-if="geolocationBlockUser">
        <p>Geolocation is blocked by user</p>
    </div>



</template>

<style scoped>

</style>
