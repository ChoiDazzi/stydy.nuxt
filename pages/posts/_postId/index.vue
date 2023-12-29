<!-- Study01Detail.vue -->

<template>
  <v-container>
    <v-card elevation="3" width="80%" class="mx-auto my-16">
      <v-card-title class="mb-12">
        <span class="mr-2"> Detail / {{ postId }} </span>
      </v-card-title>
      <v-card-text>
        <v-row>
          <v-col class="my-0">
            <v-text-field
              v-model="post.postTtl"
              label="제목"
              readonly
              variant="underlined"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row class="my-0">
          <v-col>
            <v-text-field
              v-model="post.userNm"
              label="작성자"
              readonly
              variant="underlined"
            ></v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              v-model="post.rgstDt"
              label="작성일시"
              readonly
              variant="underlined"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row class="my-0">
          <v-layout v-if="files.length != 0" row wrap>
            <v-flex v-for="file in files" :key="file.fileId" xs4>
              <a :href="`/api/v1/posts/${file.fileId}/download`" download>{{ file.fileOrgNm }}</a>
              <br/>
              <img
                :src="`/api/v1/files/${file.fileId}`"
                class="image"
                alt="lorem"
                width="100px"
                height="100%"
              />
            </v-flex>
          </v-layout>
          <v-layout v-else>
            <v-flex>등록된 첨부파일이 없습니다.</v-flex>
          </v-layout>
        </v-row>
        <v-row class="my-0">
          <v-col>
            <v-textarea
              v-model="post.postCnt"
              label="내용"
              readonly
              variant="underlined"
            ></v-textarea>
          </v-col>
        </v-row>
      </v-card-text>

      <v-card-actions>
        <div class="mx-auto">
            <v-btn class="ma-2" color="primary" elevation="3" @click="$router.push(`/posts/${postId}/edit`)">
                Edit
                <v-icon end icon="mdi-checkbox-marked-circle"></v-icon>
            </v-btn>
            <v-btn class="ma-2" color="red" elevation="3" @click="deletePost">
                Delete
                <v-icon end icon="mdi-cancel"></v-icon>
            </v-btn>
            <v-btn class="ma-2" color="grey" elevation="3" @click="this.$router.go(-1)">
                List
                <v-icon end icon="mdi-cancel"></v-icon>
            </v-btn>
        </div>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      postId: this.$route.params.postId,
      post: {},
      files: [],
    };
  },
  mounted() {
    this.$axios
      .get(`/api/v1/posts/${this.postId}`)
      .then((res) => {
        this.post = res.data.postInfo;
        this.files = res.data.fileInfo;
      })
      .catch((error) => {
        console.error("포스트 상세정보 오류:", error);
      });
  },
  methods: {
     deletePost() {
      if (confirm("게시물을 삭제하시겠습니까?")) {
        this.$axios
          .delete(`/api/v1/posts/${this.postId}`)
          .then(() => {
            this.$router.push("/board");
          })
          .catch((error) => {
            console.error("포스트 삭제 오류:", error);
          });
      }
    },
  },
};
</script>
