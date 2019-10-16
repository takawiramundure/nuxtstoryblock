<template>
  <div id="posts">
    <PostPreview
    v-for="post in posts"
    :key="post.id"
    :title="post.title"
    :previewText="post.previewText"
    :thumbnailUrl="post.thumbnailUrl"
    :id="post.id"/>
  </div>
</template>

<script>
import PostPreview from '@/components/Blog/PostPreview'

export default {
  components: {
    PostPreview
  },
  asyncData(context) {
    return context.app.$storyapi.get('cdn/stories', {
      version: context.isDev ? 'draft': 'published',
      starts_with: 'blog/'
    })
    .then(res => {
      // console.log(res)
      return {
        posts: res.data.stories.map(bp => {
        return {
          id: bp.slug,
          title: bp.content.title,
          previewText: bp.content.summary,
          thumbnailUrl: bp.content.thumbnail
        }
      })}
    })
    .catch(e => console.log('error', e))
  }
  // data() {
  //   return {
  //     posts: [
  //       {
  //         title: 'first title',
  //         previewText: 'this is the previre text for the for FIRST item',
  //         thumbnailUrl: 'https://images.pexels.com/photos/3039385/pexels-photo-3039385.png?cs=srgb&dl=alone-beach-beautiful-woman-3039385.jpg&fm=jpg',
  //         id: 'a-first-article'
  //       },
  //       {
  //         title: 'second title',
  //         previewText: 'this is the previre text for the SECOND item',
  //         thumbnailUrl: 'https://images.pexels.com/photos/3039385/pexels-photo-3039385.png?cs=srgb&dl=alone-beach-beautiful-woman-3039385.jpg&fm=jpg',
  //         id: 'a-second-article'
  //       }
  //     ]
  //   }
  // }
}
</script>

<style scoped>
  #posts {
    padding-top: 2rem;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

@media (min-width: 35rem) {
  #posts {
    flex-direction: row;
  }
}
</style>