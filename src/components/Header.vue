<template>
    <div id="mainDiv">
        <b-card-header header-tag="nav" class="border-info">
            <b-nav align="center">
                <b-nav-item disabled id="title"><span class="text-light">Check interesting things from NASA databases!</span></b-nav-item>
                <b-nav-item disabled><span class="text-danger">If you're using a mobile device we recommend you to rotate the screen - otherwise some data won't be available for you!</span></b-nav-item>
            </b-nav>
        </b-card-header>
    </div>
</template>

<script>
    export default {
        name: "Header"
    }
</script>

<style scoped>
    #title {
        font-size: 2.1em;
    }
</style>