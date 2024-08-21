<template>
    <h1>{{ title }}</h1>
    <video :src="`http://localhost:3000/uploads/${fileName}.mp4`" width="1080px" 
    controls></video>
</template>

<script>
export default {
    name: 'VideoPlayer',
    props: {
        fileName: String,
        title: String
    }
}
</script>