<template>
  <div class="container">
    <div class="comment-feed">
      <a>{{ total_comments }} Comments</a>
      <!-- form -->
      <div class="comment-feed-comments">
        <div
          class="comments-list"
          v-for="(comment, index) in comments"
          :key="index"
          @mouseover="selectIndex(index)"
          @mouseout="itemIndex = null"
        >
          <p>
            <strong class="username">{{ username }}: </strong>{{ commentDate }}
          </p>
          <div class="comment">
            {{ comment.comment }}
          </div>

          <div class="action-btns" v-show="itemIndex == index">
            <form v-on:submit.prevent="remove(index)">
              <button class="btn-comment-delete-edit">
                <svg width="2vw" height="2vh" viewBox="0 0 32 32">
                  <path
                    d="M29.98 6.819c-0.096-1.57-1.387-2.816-2.98-2.816h-3v-1.002c0-1.657-1.344-3-3-3h-10c-1.657 0-3 1.343-3 3v1.001h-3c-1.595 0-2.885 1.246-2.981 2.816h-0.019v2.183c0 1.104 0.896 2 2 2v0 17c0 2.209 1.791 4 4 4h16c2.209 0 4-1.791 4-4v-17c1.104 0 2-0.896 2-2v-2.182h-0.020zM10 3.002c0-0.553 0.447-1 1-1h10c0.553 0 1 0.447 1 1v1h-12v-1zM26 28.002c0 1.102-0.898 2-2 2h-16c-1.103 0-2-0.898-2-2v-17h20v17zM28 8.001v1h-24v-1.999c0-0.553 0.447-1 1-1h22c0.553 0 1 0.447 1 1v0.999zM9 28.006h2c0.553 0 1-0.447 1-1v-13c0-0.553-0.447-1-1-1h-2c-0.553 0-1 0.447-1 1v13c0 0.553 0.447 1 1 1zM9 14.005h2v13h-2v-13zM15 28.006h2c0.553 0 1-0.447 1-1v-13c0-0.553-0.447-1-1-1h-2c-0.553 0-1 0.447-1 1v13c0 0.553 0.447 1 1 1zM15 14.005h2v13h-2v-13zM21 28.006h2c0.553 0 1-0.447 1-1v-13c0-0.553-0.447-1-1-1h-2c-0.553 0-1 0.447-1 1v13c0 0.553 0.447 1 1 1zM21 14.005h2v13h-2v-13z"
                  />
                </svg>
              </button>
            </form>
            <path
              d="M29.98 6.819c-0.096-1.57-1.387-2.816-2.98-2.816h-3v-1.002c0-1.657-1.344-3-3-3h-10c-1.657 0-3 1.343-3 3v1.001h-3c-1.595 0-2.885 1.246-2.981 2.816h-0.019v2.183c0 1.104 0.896 2 2 2v0 17c0 2.209 1.791 4 4 4h16c2.209 0 4-1.791 4-4v-17c1.104 0 2-0.896 2-2v-2.182h-0.020zM10 3.002c0-0.553 0.447-1 1-1h10c0.553 0 1 0.447 1 1v1h-12v-1zM26 28.002c0 1.102-0.898 2-2 2h-16c-1.103 0-2-0.898-2-2v-17h20v17zM28 8.001v1h-24v-1.999c0-0.553 0.447-1 1-1h22c0.553 0 1 0.447 1 1v0.999zM9 28.006h2c0.553 0 1-0.447 1-1v-13c0-0.553-0.447-1-1-1h-2c-0.553 0-1 0.447-1 1v13c0 0.553 0.447 1 1 1zM9 14.005h2v13h-2v-13zM15 28.006h2c0.553 0 1-0.447 1-1v-13c0-0.553-0.447-1-1-1h-2c-0.553 0-1 0.447-1 1v13c0 0.553 0.447 1 1 1zM15 14.005h2v13h-2v-13zM21 28.006h2c0.553 0 1-0.447 1-1v-13c0-0.553-0.447-1-1-1h-2c-0.553 0-1 0.447-1 1v13c0 0.553 0.447 1 1 1zM21 14.005h2v13h-2v-13z"
            ></path>
            <div @click="edit(index)" v-on:click="editingHide = !editingHide">
              <button class="btn-comment-delete-edit">
                <svg width="2vw" height="2vh" viewBox="0 0 22 22">
                  <path
                    d="M17.561 2.439c-1.442-1.443-2.525-1.227-2.525-1.227l-12.826 12.825-1.010 4.762 4.763-1.010 12.826-12.823c-0.001 0 0.216-1.083-1.228-2.527zM5.68 17.217l-1.624 0.35c-0.156-0.293-0.345-0.586-0.69-0.932s-0.639-0.533-0.932-0.691l0.35-1.623 0.47-0.469c0 0 0.883 0.018 1.881 1.016 0.997 0.996 1.016 1.881 1.016 1.881l-0.471 0.468z"
                  />
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-if="editingHide">
      <form class="comment-feed-create" v-on:submit.prevent="submit">
        <div>
          <textarea
            minlength="1"
            required="required"
            placeholder="Your comment"
            v-model="commentInput"
            class="comment-feed-input-form"
          ></textarea>
        </div>
        <button class="btn-create-comment">
          <base-icon>
            <svg width="3.5vw" height="3.5vh" viewBox="0 0 24 24">
              <path fill="none" d="M0 0h24v24H0V0z" />
              <path
                d="M3.4 20.4l17.45-7.48c.81-.35.81-1.49 0-1.84L3.4 3.6c-.66-.29-1.39.2-1.39.91L2 9.12c0 .5.37.93.87.99L17 12 2.87 13.88c-.5.07-.87.5-.87 1l.01 4.61c0 .71.73 1.2 1.39.91z"
              />
            </svg>
          </base-icon>
        </button>
      </form>
    </div>
    <div comment_id="inputForm" v-show="!editingHide">
      <form class="comment-feed-create" v-on:submit.prevent="submitEdit(index)">
        <div>
          <textarea
            minlength="1"
            required="required"
            placeholder="Editing massage"
            v-model="editInput"
            class="comment-feed-input-form"
          ></textarea>
        </div>

        <button
          class="btn-create-comment"
          v-on:click="editingHide = !editingHide"
        >
          <base-icon viewBox="0 0 970 970">
            <svg width="3.5vw" height="3.5vh" viewBox="0 0 24 24">
              <path fill="none" d="M0 0h24v24H0V0z" />
              <path
                d="M3.4 20.4l17.45-7.48c.81-.35.81-1.49 0-1.84L3.4 3.6c-.66-.29-1.39.2-1.39.91L2 9.12c0 .5.37.93.87.99L17 12 2.87 13.88c-.5.07-.87.5-.87 1l.01 4.61c0 .71.73 1.2 1.39.91z"
              />
            </svg>
          </base-icon>
        </button>
      </form>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      record: "",
      commentInput: "",
      editInput: "",
      itemIndex: null,
      total_comments: 0,
      comment_id: "",
      editingHide: true,
      comments: [
        {
          comment:
            "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industrys standard dummy text ever since the when an unknown printer took a galley of type and scrambled it to make a type specimen book.",
        },
        {
          comment:
            "Lorem Ipsum is simply dummy text of the printing and typesetting industry.",
        },
      ],
    };
  },
  computed: {
    username() {
      return "Firstname  Lastname";
    },
    commentDate() {
      return (
        new Date().toISOString().slice(0, 10) +
        " " +
        new Date().toISOString().slice(11, 16)
      );
    },
  },
  methods: {
    getSingleTon: function (comment) {
      return function () {
        return {
          comment,
        };
      };
    },
    selectIndex(index) {
      this.itemIndex = index;
    },
    submit() {
      let comment = this.getSingleTon(this.commentInput);
      let old = this.comments;
      let newc = [...old, comment()];
      this.comments = newc;
      this.commentInput = "";
      this.comments.forEach((item, i) => {
        item.comment_id = i + 1;
      });
      this.total_comments = this.comments.length;
    },
    remove(index) {
      const removeId = this.comments[index].comment_id;
      function removeObjectWithId(arr, comment_id) {
        const objWithIdIndex = arr.findIndex(
          (obj) => obj.comment_id === comment_id
        );
        arr.splice(objWithIdIndex, 1);
        return arr;
      }
      removeObjectWithId(this.comments, removeId);
      this.total_comments = this.comments.length;
    },
    edit(index) {
      this.comment_id = index;
      this.editInput = this.comments[index].comment;
    },
    submitEdit() {
      let comment_id = this.comment_id;
      this.comments[comment_id].comment = this.editInput;
      this.editInput = "";
    },
  },
};
</script>

<style>
.container {
  position: absolute;
  font-family: Satoshi, sans-serif;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 70%;
  background-color: #99FF99;
  box-shadow: 0 1rem 2rem rgb(0 0 0 / 10%);
  border-radius: 5px;
  display: table;
}
.comment-feed {
  padding: 2vw;
  border-radius: 10px;
  color: #2e2e2e;
  overflow: hidden;
}
.comments-list {
  line-height: 2rem;
}
.username {
  font-size: 110%;
}
.comment {
  margin-top: -2vh;
  font-size: 92%;
}
.comment-feed-input-form {
  font-family: Satoshi, sans-serif;
  left: 100px;
  resize: none;
  height: 5vh;
  width: 100%;
  border: 0;
  padding: 1vh 0 0 1vw;
  border-radius: 5px;
}
.comment-feed-comments {
  padding: 5px;
  height: 50vh;
  width: 100%;
  overflow-y: auto;
  word-break: break-all;
}
.comment-feed-create {
  position: relative;
  margin-top: 2vw;
  width: 85%;
  left: 2vw;
  bottom: 1.6vh;
}
.action-btns {
  display: flex;
  flex-direction: row;
}
.btn-create-comment {
  background-color: transparent;
  position: fixed;
  outline: none;
  border: 0;
  bottom: 2.7vh;
  right: 1.5vw;
  border: 0;
  cursor: pointer;
}
.btn-comment-delete-edit {
  background-color: transparent;
  border: 0;
  cursor: pointer;
  border-radius: 5px;
}
</style>
