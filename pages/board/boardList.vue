<template>
    <v-container>
        <v-sheet elevation="1">
            <v-row>
                <v-col cols="12" md="5">
                    <v-text-field v-model="srchData.type" label="타입" density="compact" variant="outlined"></v-text-field>
                </v-col>
                <v-col cols="12" md="5">
                    <v-text-field v-model="srchData.keyword" label="검색내용" density="compact" variant="outlined"></v-text-field>
                </v-col>
                <v-col cols="12" md="5">
                    <v-btn class="mt-2 mr-2" color="primary" @click="search">검색</v-btn>
                    <v-btn class="mt-2" color="primary" @click="write">등록</v-btn>
                </v-col>
            </v-row>
        </v-sheet>
        <v-sheet>
            <v-table>
                <thead>
                    <tr>
                        <th>#</th>
                        <th>ID</th>
                        <th>제목</th>
                        <th>작성자</th>
                        <th>작성일</th>
                    </tr>
                </thead>
                <tbody>
                    <template v-if="true">
                        <tr v-for="(post, idx) in posts" :key="post.postId">
                            <td>{{ idx + 1 }}</td>
                            <td><nuxt-link :to="{ path: `/posts/${post.postId}` }">{{ post.postId }}</nuxt-link></td>
                            <td>{{ post.postTtl }}</td>
                            <td>{{ post.userNm }}</td>
                            <td>{{ post.rgstDt }}</td>
                        </tr>
                    </template>
                    <template v-else>
                        <tr>
                            <td colspan="5">등록된 게시물이 없습니다.</td>
                        </tr>
                    </template>
                </tbody>
            </v-table>
        </v-sheet>
    </v-container>
</template>

<script>
export default {
  data() {
    return {
      posts: [],
      srchData: {
        type: '',
        keyword: '',
      },
    };
  },
  mounted() {
    this.$axios
      .get('/api/v1/boards')
      .then((resp) => {
        this.posts = resp.data;
        console.log(this.posts);
      })
      .catch(() => {
        console.error(arguments);
      });
  },
  methods: {
    async search() {
      try {
        const resp = await this.$axios.get('/api/v1/boards', {
          params: this.srchData,
        });
        this.posts = resp.data;
      } catch (e) {
        console.log(e);
      }
    },
    async write() {
      await this.$router.push('/posts/write');
    },
  },
};
</script>
