<template>
    <img :src="`http://openweathermap.org/img/wn/${icon}@2x.png`" />
</template>

<script>
    export default{
        props: ["icon"]
    }
</script>