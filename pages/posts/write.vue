<template>
  <v-container>
    <v-card elevation="3" width="80%" class="mx-auto my-16">
      <v-card-title class="mb-12">
        <span class="mr-2"> Write </span>
      </v-card-title>
      <v-card-text>
        <v-row>
          <v-col class="my-0">
            <v-text-field label="제목" variant="underlined" v-model="postVO.postTtl"></v-text-field>
          </v-col>
        </v-row>
        <v-row class="my-0">
          <v-col>
            <v-file-input multiple label="첨부파일" variant="underlined" v-model="files"></v-file-input>
          </v-col>
        </v-row>
        <v-row class="my-0">
          <v-col>
            <v-textarea label="내용" variant="underlined" v-model="postVO.postCnt"></v-textarea>
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions>
        <div class="mx-auto">
          <v-btn @click="register" class="ma-2" color="primary" elevation="3">
            SAVE
            <v-icon end icon="mdi-checkbox-marked-circle"></v-icon>
          </v-btn>
          <v-btn @click="this.$router.go(-1)" class="ma-2" color="grey" elevation="3">
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
      postVO: {
        postTtl: '',
        postCnt: '',
      },
      files: []
    }
  },
  methods: {
    async register() {
      try {
        const formData = new FormData();
        formData.append("postVO", JSON.stringify(this.postVO));

        for (let i = 0; i < this.files.length; i++) {
          formData.append("files", this.files[i]);
        }

        await this.$axios
          .post('/api/v1/posts', formData, {
            Headers: 'Content-Type: multipart/form-data'
          })
          .then(() => {
            this.$router.go(-1);
          });
      } catch (error) {
        console.error("포스트 등록 중 오류: ", error);
      }
    }
  }
}
</script>
