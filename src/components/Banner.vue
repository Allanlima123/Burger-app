<template>
    <section id="main--benner">
        <h1>Make Your Burger</h1>
    </section>
</template>

<script>
export default {
   name : "Banner"
}
</script>

<style scoped>
    #main--benner{
        background-image: url('../../public/img/burger.jpg');
        background-position: 0 -250px;
        min-height: 500px;
        background-repeat: no-repeat;
        background-size: cover;
        display: flex;
        position: relative;
        align-items: center;
        justify-content: flex-start;
    }

    #main--benner h1{
        color: #fff;
        text-align: center;
        font-size: 60px;
        background: #222;
        padding: 20px 40px;
    }
</style>