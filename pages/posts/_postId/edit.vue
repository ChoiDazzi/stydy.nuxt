<template>
  <v-container>
    <v-card elevation="3" width="80%" class="mx-auto my-16">
      <v-card-title class="mb-12">
        <span class="mr-2"> Edit / {{ postId }} </span>
      </v-card-title>
      <v-card-text>
        <v-row>
          <v-col class="my-0">
            <v-text-field v-model="post.postTtl" label="제목" variant="underlined"></v-text-field>
          </v-col>
        </v-row>
        <v-row class="my-0">
          <v-col>
            <v-text-field v-model="post.userNm" label="작성자" readonly variant="underlined"></v-text-field>
          </v-col>
          <v-col>
            <v-text-field v-model="post.rgstDt" label="작성일시" readonly variant="underlined"></v-text-field>
          </v-col>
        </v-row>
        <v-row class="my-0">
          <v-col>
            <v-file-input v-model="files" multiple label="첨부파일" variant="underlined"></v-file-input>
          </v-col>
        </v-row>
        <v-row class="my-0">
          <v-layout v-if="fileInfo.length != 0" row wrap>
            <v-flex v-for="file in fileInfo" :key="file.fileId" xs4 class="mr-3">
              <span class="mr-1">{{ file.fileOrgNm }}</span>
              <br>
              <img :src="`/api/v1/files/${file.fileId}`" class="image" alt="lorem" width="100px" height="100%" />
            </v-flex>
          </v-layout>
          <v-layout v-else>
            <v-flex>등록된 첨부파일이 없습니다.</v-flex>
          </v-layout>
        </v-row>
        <v-row class="my-0">
          <v-col>
            <v-textarea v-model="post.postCnt" label="내용" variant="underlined"></v-textarea>
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions>
        <div class="mx-auto">
          <v-btn @click="edit" class="ma-2" color="primary" elevation="3">
            SAVE
            <v-icon end icon="mdi-checkbox-marked-circle"></v-icon>
          </v-btn>
          <v-btn @click="this.$router.go(-1)" class="ma-2" color="grey" elevation="3">
            CANCEL
            <v-icon end icon="mdi-arrow-left"></v-icon>
          </v-btn>
        </div>
      </v-card-actions>
    </v-card>
  </v-container>
</template>
<script>
export default {
  props: ["postId"],
  data() {
    return {
      post: {},
      fileInfo: [],
      files: [],
    };
  },
  mounted() {
    this.$axios
      .get(`/api/v1/posts/${this.postId}`)
      .then((res) => {
        this.post = res.data.postInfo;
        this.fileInfo = res.data.fileInfo;
      })
      .catch((error) => {
        console.error("포스트 상세 정보 오류:", error);
      });
  },
  methods: {
    async edit() {
      try {
        const formData = new FormData();
        formData.append("postVO", JSON.stringify(this.post));

        for (let i = 0; i < this.files.length; i++) {
          formData.append("files", this.files[i], this.files[i].name);
        }

        await this.$axios
          .put(`/api/v1/posts`, formData, {
            headers: {
              "Content-Type": "multipart/form-data",
            },
          })
          .then(this.$router.push(`/posts/${this.postId}`));
      } catch (error) {
        console.error("포스트 수정 중 오류:", error);
      }
    },
  },
};
</script>
