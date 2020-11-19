<template>
    <div>
        <b-nav>
            <b-nav-item disable> <b> Fancy Quiz VueApp </b></b-nav-item>
            <b-nav-item disable> Counter: {{ numCorrect }} / {{ numTotal }} </b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal'
    ]
}
</script>

<style scoped>
div{
    margin-left: 35%;
    font-size: 22px;
}
</style>