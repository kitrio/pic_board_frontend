<template>
  <div
    class="postlist"
    grid-list-md
  >
    <v-layout
      row
      wrap
    >
      <v-flex
        v-for="(item, index) in postProps"
        :key="index"
        xs12
        sm6
        md4
        lg2
        x3
      >
        <v-hover
          v-slot:default="{ hover }"
          open-delay="180"
        >
          <v-card
            :elevation="hover ? 12 : 2"
          >
            <router-link :to="{ name: 'Contents', params: { num: item.boardNum}}">
              <v-img
                :src="`${imgPath}${item.fileAltName}`"
                max-width="450px"
                aspect-ratio="1.4"
                class="white--text align-end"
                gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.3)"
              >
                <v-card-text
                  v-if="hover"
                  class="v-card-hover"
                >
                  {{ item.title }}
                  <div>
                    <v-icon
                      color="#ccc"
                    >
                      mdi-heart-outline
                    </v-icon>
                    {{ item.goodCount }}
                  </div>
                </v-card-text>
              </v-img>
            </router-link>
          </v-card>
        </v-hover>
      </v-flex>
    </v-layout>
  </div>
</template>
<script>

export default {
  name: 'PostList',
  props: {
    postProps: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {
      imgPath: process.env.VUE_APP_FILE_URL + 'thumb_'
    }
  }
}
</script>
<style scoped>
  .v-card-hover{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }
</style>
