<template>
    <div>
        <div class="col-lg-auto">
            <div class="home">
                <div class="row justify-content-lg-center row-cols-1 row-cols-md-3 g-4">
                    <div class="col-lg d-flex justify-content-center" v-for="character in charactersName"
                        :key="character.id">
                        <div class="card" style="width: 15rem;">
                            <img :src="character.image" class="card-img-top" alt="...">
                            <div class="card-body">
                                <button @click="buttonClickHandler(character.id)" type="button" class="btn btn-light">{{
                                        character.name
                                }}</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
export default {
    name: 'ListAllCharacters.vue',
    props: {
        charactersName: [],
    },

    methods: {
        buttonClickHandler(value) {
            this.$emit('my-event', value)
        }
    }
};
</script>
