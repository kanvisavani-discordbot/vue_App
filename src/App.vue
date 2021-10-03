<template>
  <div>
    <div v-show="!seen" v-for="post in posts" v-bind:key="post.id">
    {{ maxVal.push(post.Random)}}
      </div>
    <b-card-group>
    <b-card class="widthOfCard mb-3" v-for="post in posts" v-bind:key="post.id">
      <b-card-header :class="[Math.max.apply(Math, maxVal) == post.Random ? 'text-danger' : 'text-primary']" >{{ post.user_name }}<b-card-title>{{ post.Random }}</b-card-title></b-card-header>
      <b-card-body>
        <b-card-img class="imageSize" :src="require(`@/assets/profileImg/${post.profile_pic}`)" />
      </b-card-body>
    </b-card>
    </b-card-group>
  </div>
</template>

<style scoped>
.widthOfCard {
  max-width: 18rem;
}
.imageSize {
  height: 200px;
  width:200px;
}
</style>

<script>
export default {
  data() {
    return {
      posts: [],
      maxVal: [],
      seen: true
    };
  },

  methods: {
    async getData() {
      try {
        const response = await this.$http.get(
            "http://localhost:4449/getUsers"
        );
        this.posts = response.data;
      } catch (error) {
        console.log(error);
      }
    },
  },

  created() {
    window.setInterval(() => {
      this.maxVal=[];
      this.getData();
    }, 1000)
  },
};
</script>