<template>
    <div id="app">
        <div class="card-container">
            <div class="card" v-for="(card, index) in foundPages" :key="index">
                <h3>{{ card.pageTitle }}</h3>
                <p class="text-limited" >{{ card.content }}</p>
                <div v-if="card.image">
                    <img :src="card.image" alt="Image" class="img-fluid" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['foundPages'],
    data() {

    }
};
</script>
