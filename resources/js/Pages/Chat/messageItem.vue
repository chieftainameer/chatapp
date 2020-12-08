<template>
    <div>{{this.message.user.name}} : {{this.message.message}}</div>
</template>
<script>
export default {
    props:['message'],
}
</script>
<style scoped>

</style>