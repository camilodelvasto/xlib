<template>
  <div>
    <div class="comments-section__wrapper">
      <div class="comments-section__comment-wrapper"
        v-for="comment in commentsTree"
        v-if="!comment.isReply"
      >
        <div class="comment-item__comment-wrapper">
          <Comment
            :comment="comment"
            :fundraiser-id="fundraiserId"
            v-if="!comment.parent"
          ></Comment>
        </div>
        <div class="comment-item__comment-replies">
          <div class="comment-item__comment-wrapper" v-for="reply in comment.children">
            <Comment :comment="reply" :is-reply="true" :fundraiser-id="fundraiserId" />
          </div>
        </div>
      </div>
    </div>
    <button
      class="button is-warning is-load-more" 
      @click="loadMoreComments()"
      v-if="moreComments"
    >Show more comments</button>
    <h4>Leave a comment</h4>
    <CommentReply class="comment-reply__wrapper" />
  </div>
</template>

<style scoped lang="scss">
.is-load-more {
  margin-bottom: 40px;
}
.comment-reply__wrapper {
  margin-left: 60px;
}

</style>

<script>
import Comment from "Components/general/Comment.vue"
import CommentReply from "Components/general/CommentReply.vue"
import arrayToTree from "array-to-tree"

export default {
  props: [ "comments", "moreComments", "fundraiserId" ],
  components: {
    Comment,
    CommentReply
  },
  data () {
    return {
      commentsTree: []
    }
  },
  mounted () {
    this.refreshTree()
  },
  methods: {
    loadMoreComments () {
      this.$emit("loadMoreComments")
    },
    /*
     * This will create the tree to be used in the comments section, so that nested comments show.
    */
    refreshTree () {
      var nested = arrayToTree(this.comments, {
        parentProperty: "parent",
        customID: "id"
      })
      this.commentsTree = nested
    }
  },
  watch: {
    comments () {
      this.refreshTree()
    }
  }
}
</script>
