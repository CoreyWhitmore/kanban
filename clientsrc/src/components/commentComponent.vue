<template>
  <div class="card bg-faded row mx-2 my-1 p-2 text-dark">
    <div v-if="!editMode" class="col d-flex justify-content-between">
      <div class="row">
        <i class="fa fa-pencil-alt mx-1 icon-pop" @click="toggleEdit" aria-hidden="true"></i>
        <h6 class="comorant">{{commentProp.title}} </h6>
      </div>
      <i class="fa fa-times align-self-start  icon-pop" @click="deleteComment" aria-hidden="true"></i>
    </div>
    <div v-else class="col d-flex flex-column justify-content-between">
      <div class="row justify-content-end">
        <div class="input-group mb-3">
          <div class="input-group-prepend">
            <button class="btn btn-secondary" @click="toggleEdit" type="button"><i class="fa fa-undo align-self-center"
                aria-hidden="true"></i></button>
          </div>
          <input type="text" class="form-control bg-light" v-model="newComment.title" :placeholder="commentProp.title">
          <div class="input-group-append">
            <button class="btn btn-secondary" @click="editComment" type="button"><i
                class="fas fa-save align-self-center"></i></button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "comment",
    props: ["commentProp"],
    data() {
      return {
        newComment: { title: this.commentProp.title },
        editMode: false,

      }
    },
    methods: {
      toggleEdit() {
        this.editMode = !this.editMode
      },
      editComment() {
        this.newComment.taskId = this.commentProp.taskId
        this.newComment.id = this.commentProp.id
        this.$store.dispatch("editComment", this.newComment)
        this.toggleEdit()
      },
      deleteComment() {
        Swal.fire({
          title: 'Are you sure you want to delete this comment?',
          text: "You won't be able to revert this!",
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: 'var(--danger)',
          cancelButtonColor: 'var(--success)',
          confirmButtonText: 'Yes, delete it!',
          background: 'var(--lighttransparent)'
        }).then((result) => {
          if (result.isConfirmed) {
            this.$store.dispatch("deleteComment", this.commentProp)
            Swal.fire({
              title: 'Deleted!',
              text: 'Your comment has been deleted.',
              confirmButtonText: 'OK',
              background: 'var(--lighttransparent)',
              icon: 'success'
            })
          }
        })
      }
    }

  }

</script>

<style>

</style>