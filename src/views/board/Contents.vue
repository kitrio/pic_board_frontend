<template>
  <v-app id="contents">
    <v-container>
      <v-card
        class="card-container"
      >
        <v-card-title v-text="postOne.title" />
        <v-card-text>
          <router-link :to="{name:'UserInfo', params:{ nickname: postOne.nickname } }">
            {{ postOne.nickname }}
          </router-link>
          | {{ postOne.writeTime }}
        </v-card-text>
        <div class="imgbox">
          <img
            v-if="postOne.fileAltName !== undefined"
            :src="`${imgPath}${postOne.fileAltName}`"
            class="image"
          >
        </div>
        <p class="text-content">
          <v-card-text>
            {{ postOne.content }}
          </v-card-text>

          <v-card-actions>
            <v-spacer />
            <v-label>
              <v-icon>mdi-eye</v-icon>
              {{ postOne.readCount }}
            </v-label>

            <v-btn
              @click="goodCounter"
            >
              <v-icon>mdi-heart</v-icon>
              <v-label>{{ postOne.goodCount }}</v-label>
            </v-btn>

            <v-btn icon>
              <v-icon>mdi-share-variant</v-icon>
            </v-btn>
          </v-card-actions>
          <v-card-actions v-if="memberInfo === postOne.memberId">
            <v-btn
              @click="modifyContent"
            >
              <v-label>수정</v-label>
            </v-btn>
            <v-btn
              @click="deleteContent"
            >
              <v-label>삭제</v-label>
            </v-btn>
          </v-card-actions>
        </p>
      </v-card>
    </v-container>
  </v-app>
</template>
<script>

import { mapGetters } from 'vuex'
export default {
  name: 'Contents',
  data () {
    return {
      imgPath: process.env.VUE_APP_FILE_URL,
      postOne: {
        nickname: ' '
      }
    }
  },
  computed: {
    ...mapGetters({
      memberInfo: (['member/getMemberId'])
    })
  },
  mounted () {
    this.content()
  },
  methods: {
    content () {
      this.$axios({
        method: 'get',
        headers: {
          Accept: 'application/json'
        },
        url: `/list/content/${this.$route.params.num}`
      })
        .then(response => {
          this.postOne = response.data
        })
        .catch(error => {
          if (error.response.status === 404) {
            alert('해당 게시물이 없습니다.')
            this.$router.push('/')
          }
        })
    },
    goodCounter () {
      this.$axios({
        method: 'put',
        url: `/list/content/good/${this.$route.params.num}`
      })
        .then(response => {
          this.postOne.goodCount += 1
          console.log(this.memberInfo)
        })
        .catch(error => {
          console.log(error)
        })
    },
    deleteContent () {
      const isDelete = confirm('정말 삭제하겠습니까?')
      if (isDelete === true) {
        this.$axios({
          method: 'delete',
          url: `/list/content/delete/${this.$route.params.num}`
        })
          .then(response => {
            alert('삭제 되었습니다.')
            this.$router.push('/')
          })
          .catch(() => {
            alert('삭제 할 수 없습니다.')
          })
      }
    },
    modifyContent () {
      this.$store.dispatch('contents/contentAction', this.postOne)
        .then(() => {
          this.$router.replace({ name: 'Editor' })
        })
    }
  }

}
</script>
<style scoped>
  .card-container{
    display: flex;
    flex-direction: column;
  }
  .text-content {
    font-size: 1.1em;
    color: black;
    font-family: Arial, Helvetica;
  }
  .imgbox {
    display: flex;
    margin: auto;
    max-width: 1000px;
  }
  .imgbox > img {
    width: 100%;
  }
</style>
