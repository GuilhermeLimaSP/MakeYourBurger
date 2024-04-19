<template>
    <div id="main-banner">
        <h1>Make Your Burguer</h1>
    </div>
</template>

<script>
export default {
    name: "Banner",
    data() {
        return {

        };
    },
}
</script>

<style scoped>
#main-banner{
    background-image: url('/public/img/burger.jpg'); 
    /* background-position: 0 -250px; */
    background-size: cover;
    height: 450px;

    display: flex;
    align-items: center;
    justify-content: flex-start;
}

#main-banner h1{
    color: #fff;
    text-align: center;
    font-size: 3rem;
    background-color: #222;
    padding: 20px 40px;
}
</style>