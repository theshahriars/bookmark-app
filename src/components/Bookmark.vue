<template>
  <div class="row">
    <div class="col-md-12 mb-3" v-for="bookmark in bookmarks" v-if="bookmarks.length > 0">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">
            {{ bookmark.title }}
            <span class="badge badge-success" v-if="bookmark.visited">Visited</span>
            <span class="badge badge-warning" v-else>Not Visited</span>
          </h5>
          <p class="card-text">{{ bookmark.note }}</p>
          <a :href="bookmark.link" target="_blank" class="btn btn-primary">Visit Website</a>
          <a href="javascript:void(0)" class="btn btn-danger" v-on:click="deleteBookmark(bookmark)">Delete Bookmark</a>
        </div>
      </div>
    </div>
    <div class="col-md-12" v-if="bookmarks.length <= 0">
      <div class="jumbotron jumbotron-fluid">
        <div class="container text-center">
          <h1>Nothing Found</h1>
          <p class="lead">Bookmark some awesome website.</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import sweetalert from 'sweetalert'
  export default {
    name: "Bookmark",
    props: ['bookmarks'],
    methods: {
      deleteBookmark(bookmark_data) {

        // getting index of bookmark
        const bookmarkIndex = this.bookmarks.indexOf(bookmark_data)

        // removing bookmarks
        this.bookmarks.splice(bookmarkIndex, 1)

        // showing alert
        sweetalert('Deleted!', 'Your bookmark has been deleted.', 'success')

        // updating localStorage data
        window.localStorage.removeItem('bookmark_app')
        window.localStorage.setItem('bookmark_app', JSON.stringify(this.bookmarks))
      }
    }
  }
</script>

<style scoped>

</style>
