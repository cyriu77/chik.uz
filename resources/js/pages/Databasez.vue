<template>
  
  <div class="container">
    <div >
   
   
    
   <div class="page__header">
   <h2 class="diz">Базы данных</h2>
   
 </div>
 
 
   
 <h2>{{ category }} </h2>
 <div class="rubrics__items ">
<div class="rubrics_item">
  <div class="card "  v-for="post in posts" :key="post.id">
  <img
    v-if="post.images.length > 0"  
    :src="post.images[0]?.url"    
    :alt="`First Image`"
    
  />
  <div class="card-body">
    <h5 class="card-title"><a href="single-blog.html"></a>
          <router-link
            :to="{
              name: 'SingleBlog',
              params: { slug: post.slug },
            }"
            >{{ post.title }}</router-link
          ></h5>
          
    <br>
    <h6 class="card-subtitle mb-2 text-end">{{ post.price }} Uzs</h6> 
    <hr>
    <div class="card-user">
      <img
    v-if="post.images.length > 0"  
    :src="post.images[0]?.url"    
    :alt="`First Image`"
    style="width: 30px; height: 30px; border-radius: 50%;"
  /><!--USER PHOTO-->
   <h6 >{{ post.title }}<!--USER NAME--></h6>
    </div>
   
    <!--<p class="card-text">{{ post.body }}</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>-->
  </div>
</div>
</div>

</div>

 
 

</div>
<div class="pagination" id="pagination">
      <a
        href="#"
        v-for="(link, index) in links"
        :key="index"
        v-html="link.label"
        :class="{ active: link.active, disabled: !link.url }"
        @click="changePage(link)"
      ></a>
    </div>
  </div>
    
  
 


</template>

<script>
export default {
props: ['posts', 'category'],

data() {
  return {
    posts: [],
    links: [],
  };
},

mounted() {
  axios
        .get("/api/categories")
        .then((response) => (this.categories = response.data))
        .catch((error) => {
          console.log(error);
        });
  this.filterByCategory('Базы данных'); // Initial load with category name 'Design'
},

methods: {
  filterByCategory(name) {
    axios
      .get("/api/posts", {
        params: {
          category: name,
        },
      })
      .then((response) => {
        this.posts = response.data.data;
        this.links = response.data.meta.links;
      })
      .catch((error) => {
        console.log(error);
      });
  },
  changePage(link) {
    if (!link.url || link.active) {
        return;
    }
    axios
        .get(link.url)
        .then((response) => {
            this.posts = response.data.data;
            this.links = response.data.meta.links;
        })
        .catch((error) => {
            console.log(error);
        });
},
},
watch: {
    title() {
      axios
        .get("/api/posts", {
          params: {
            search: this.title,
          },
        })
        .then((response) => {
          this.posts = response.data.data;
          this.links = response.data.meta.links;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style>
.right_side_menu{
  color: rgb(0, 0, 0);
}
.design_page{
  display: flex;
  margin-top: 40px;
 
}
.col-md-8{
  width: 60vw;
}
.page__header{

margin: 40px 0 34px;
  text-align: center;
}
.item_title{
  
  color: #111;
    font-size: 14px;
    height: 36px;
    line-height: 18px;
    margin-bottom: 8px;
    max-height: 100%;
    min-height: auto;
    padding: 0;
}
.item-price{
  
  height: 50px;
    padding: 13px 0 0;
  color: #04b70a;
    font-size: 16px;
    font-weight: 700;
}
.diz{
display: block;
  font-size: 2em;
  margin-block-start: 0.67em;
  margin-block-end: 0.67em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
  font-weight: bold;
}
.card-blog-content{
  box-sizing: border-box;
    padding: 6px;
    width: 25%;
}
.card_item{
  -webkit-box-align: center;
    -ms-flex-align: center;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    align-items: center;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
    flex-wrap: wrap;
    justify-content: center;
    margin: 13px -6px 0;

}
@media (min-width: 768px){
  .rubrics_item{
    width: 25%;
  }
}
@media (max-width: 768px){
  .rubrics_item{
    width: 50%;
  }
}
</style>
