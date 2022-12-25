<template>
  <div class="body">
    <div class="container">
      <div class="input-group mb-3">
        <div class="container search">
          <div class="row justify-content-lg-center">
            <div class="col d-flex justify-content-center">
              <input  @keydown.enter="onSubmit1" type="text" class="form-control" placeholder="Search" aria-label="Search"
                aria-describedby="button-addon2" v-model="search">
              <button @click="onSubmit1" class="btn btn-outline-secondary" type="button"
                id="button-addon2">Search</button>
            </div>
            <div class="row justify-content-lg-center">
              <div class="col d-flex justify-content-center">
                <el-pagination layout="prev, pager, next" :total="this.page_total"
                  v-model:current-page="this.pages_select" @click="onSubmit1" />
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row justify-content-lg-center">
        <ListAllCharacters v-if="alone === false" :charactersName="charactersName" @my-event="onSubmit2" />
        <CharacterById v-else :charactersName="charactersName" />
      </div>
    </div>
  </div>
  <div class="input-group mb-3">
    <div class="container search">
      <div class="row justify-content-lg-center">
        <div class="col d-flex justify-content-center">
          <el-pagination layout="prev, pager, next" :total="this.page_total" v-model:current-page="this.pages_select"
            @click="onSubmit" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { ElLoading } from 'element-plus'
import ListAllCharacters from '@/components/ListAllCharacters.vue';
import CharacterById from '@/components/CharacterById.vue';
export default {

  components: {
    ListAllCharacters,
    CharacterById
  },

  data() {
    return {
      pages_select: 1,
      charactersName: "",
      page_total: 1,
      search: "",
      fullscreenLoading: false,
      alone: false
    }
  },


  created() {
    this.getSearchCharacters(this.search, this.pages_select);
  },

  methods: {

    onSubmit1() {
      this.alone = false
      this.getSearchCharacters(this.search, this.pages_select)
    },

    onSubmit2(value) {
      this.getCharactersByid(value)
    },


    async getCharactersByid(value) {

      const loadingInstance = ElLoading.service()
      const res = await axios.post('https://rickandmortyapi.com/graphql', {
        query: `query($id_select: ID!)  {
        character(id: $id_select) {
          id
          name
          status
          species
          gender
          image
        episode {
          id
          name
        }
        }
        
      }`, variables: {
          id_select: value
        }
      })

      this.charactersName = res.data.data
      this.page_total = 1
      loadingInstance.close()
      this.alone = true

    },


    async getSearchCharacters(value, value1) {
      const loadingInstance = ElLoading.service()
      const res = await axios.post('https://rickandmortyapi.com/graphql', {
        query: ` query($pagesSelect: Int!, $nameSelect : String!) {
          characters(page: $pagesSelect, filter: { name: $nameSelect }) {
            info {
              count
              pages
            }
            results {
                id
                name
                status
                species
                gender
                image
            }
          
 
          }}`, variables: {
          nameSelect: value,
          pagesSelect: value1

        }
      })

      this.charactersName = res.data.data.characters.results
      this.page_total = res.data.data.characters.info.pages * 10
      loadingInstance.close()
    },




  }

};


</script>

<style>
.card {
  align-items: center;
}

.container .search {

  padding: 10px;
}
</style>
