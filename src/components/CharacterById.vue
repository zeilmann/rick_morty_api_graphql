<template>
    <div>
        <div class="col-lg-auto">
          <div class="home">
            <div class="row justify-content-lg-center row-cols-1 row-cols-md-3 g-4">
              <div class="col-lg d-flex justify-content-center" v-for="character in charactersName" :key="character.id">
                <div class="card" style="width: 15rem;">
                  <img :src="character.image" class="card-img-top" alt="...">
                  <div class="card-body">
                    <button @click="onSubmit2(character.id)" type="button" class="btn btn-light">{{ character.name
                    }}</button>
                    <div class="status">
                      <br>
                      <span v-if="character.status === 'Dead'" class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">
                        DEAD
                         </span>
                      <span v-else class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-primary">
                        ALIVE
                        </span>
                       <span>Gender: {{ character.gender }}</span>
                      <br>
                      <span>Specie: {{ character.status }}</span>
                      <br>
                      <br>
                      <hr>
                      <h5>All Episodes: </h5>
                      <div v-for="character in charactersName.character.episode" :key="character.id">
                        <span>{{ character.name}}</span>
                        <hr>
                      </div>
                    </div>
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
    name: 'CharacterById.vue',
    props: {
        charactersName: [],
    },

    
};
</script>