<template>
  <div id="add-blog">
    <h2>Add a New Blog Post</h2>
    <form v-if="!submitted">
      <label>Blog Title:</label>
      <input type="text" v-model.lazy="blog.title" required />
      <label>Blog Content:</label>
      <textarea v-model.lazy="blog.content"></textarea>
      <div id="checkboxes">
        <label>Ninjas</label>
        <input type="checkbox" value="ninjas" v-model="blog.categories" />
        <label>Wizards</label>
        <input type="checkbox" value="wizards" v-model="blog.categories" />
        <label>Mario</label>
        <input type="checkbox" value="mario" v-model="blog.categories" />
        <label>Cheese</label>
        <input type="checkbox" value="cheese" v-model="blog.categories" />
      </div>
      <label>Author:</label>
      <select v-model="blog.author">
        <option v-for="author in authors" :key="author">
          {{ author }}
        </option>
      </select>
      <button @click.prevent="post">Add Blog</button>
    </form>
    <div v-if="submitted">
      <h3>Thanks For Adding Your Post</h3>
    </div>
    <div id="preview">
      <h3><b>Preview Blog</b></h3>
      <p><b>Blog title:</b></p>
      <p>{{ blog.title }}</p>
      <p><b>Blog content:</b></p>
      <p>{{ blog.content }}</p>
      <p><b>Blog Categories:</b></p>
      <ul>
        <li v-for="category in blog.categories" :key="category">
          {{ category }}
        </li>
      </ul>
      <p><b>Author:</b> {{ blog.author }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      blog: {
        title: "",
        content: "",
        categories: [],
        author: "",
      },
      authors: ["The Net Ninja", "The Angular Avenger", "The Vue Vindicator"],
      submitted: false,
    };
  },
  methods: {
    post() {
      this.$http
        .post(
          "https://vue2-blog-project-default-rtdb.firebaseio.com/posts.json",
          this.blog
        )
        .then(function (data) {
          console.log(data);
          this.submitted = true;
        }); // then method kullanıyoruz çünkü bu bir promise. (.post ile beraber bir promise döner.)
    },
    // post() {
    //   this.$http
    //     .post("http://jsonplaceholder.typicode.com/posts", {
    //       // Bu link'e post request atıyoruz. (post request to fake json server)
    //       title: this.blog.title,
    //       body: this.blog.content,
    //       userId: 1,
    //       // Bunlar da ne göndermek istediklerimiz...
    //     })
    //     .then(function (data) {
    //       console.log(data);
    //       this.submitted = true;
    //     }); // then method kullanıyoruz çünkü bu bir promise. (.post ile beraber bir promise döner.)
    // },
  },
};
</script>

<style scoped>
#add-blog * {
  box-sizing: border-box;
}
#add-blog {
  margin: 20px auto;
  max-width: 500px;
}
label {
  display: block;
  margin: 20px 0 10px;
}
input[type="text"],
textarea {
  display: block;
  width: 100%;
  padding: 8px;
}
#preview {
  padding: 10px 20px;
  border: 1px dotted #ccc;
  margin: 30px 0;
  text-align: left;
}
h3 {
  margin-top: 10px;
}
#preview ul {
  padding: 0;
  margin-left: 0;
  text-align: left;
}

#preview li {
  margin-left: 0;
}

#checkboxes input {
  display: inline-block;
  margin-right: 10px;
}

#checkboxes label {
  display: inline-block;
}
</style>
