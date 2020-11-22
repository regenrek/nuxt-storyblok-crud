<template>
  <div class="mx-auto max-w-2xl my-5">
    <div>
      <h1 class="text-4xl">
        {{ home.name }}
      </h1>

      <div class="bg-gray-100 py-8 px-8 my-8">
        <ul class="grid gap-y-4">
          <li v-for="(post, index) in posts" :key="index" class="">
            {{ index + 1 }}. {{ post.content.title }}
          </li>
        </ul>

        <div class="mt-5" style="height: 20px">
          <span v-show="$fetchState.pending" class="italic"
            >Posts are loading...</span
          >
        </div>
      </div>

      <div class="flex space-x-4">
        <button
          class="focus:outline-none bg-green-500 active:bg-green-700 font-semibold rounded-md text-white py-2 px-3 shadow-md"
          @click="createNewPost"
        >
          Create new post
        </button>
        <button
          class="focus:outline-none bg-blue-500 active:bg-blue-700 font-semibold rounded-md text-white py-2 px-3 shadow-md"
          @click="refetchPosts"
        >
          Refetch Posts
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import slugify from 'slugify'
const PARENT_STORY_ID = '<insert your parent id here>'

export default {
  async asyncData({ $storyblok }) {
    try {
      const { story } = await $storyblok.getCurrentStory()
      return {
        home: story
      }
    } catch (ex) {
      console.error("Couldn't fetch home data", ex)
    }
  },
  data() {
    return {
      posts: []
    }
  },
  async fetch() {
    try {
      const { stories } = await this.$storyblok.getStoryCollection('posts')

      this.posts = stories
    } catch (ex) {
      console.error("Couldn't fetch posts", ex)
    }
  },
  methods: {
    createNewPost() {
      const title = `My ${this.getRand(100, 999)} post`
      this.$sbManagement.createStory(
        {
          name: title,
          slug: this.toSlug(title),
          parent_id: PARENT_STORY_ID,
          content: {
            title: title,
            component: 'post'
          }
        },
        { publish: 1 }
      )
    },
    refetchPosts() {
      this.$fetch()
    },
    getRand(min, max) {
      let randomNum = Math.random() * (max - min) + min
      return Math.floor(randomNum)
    },
    toSlug(title) {
      return slugify(title, {
        remove: /[*+~.,()'"!:@]/g,
        lower: true,
        locale: 'de'
      })
    }
  }
}
</script>
