<template>
    <NavBar :logo_src="logo_src" :app_name="app_name" />
    <router-view />
    <Footer />
</template>

<script>
import NavBar from '@/components/NavBar.vue';
import Footer from '@/components/Footer.vue';

export default {
    name: 'App',
    components: {
        NavBar,
        Footer
    }, 
    data() {
        return {
            app_name: "MakeYourBurguer",
            logo_src: "/img/logo.png",
        }
    }
}

</script>

<style>
 *{
    font-family: Helvetica;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
 }

 .main-container {
    margin: 50px;
    min-height: 250px;
 }

 h1 {
    text-align: center;
    font-size: 24px;
    margin-bottom: 30px;
    color: #222;
 }
</style>
