<template>
    <div>
        <div class="container">
            <div class="blog-single gray-bg">
        <div class="container">
            <div class="row align-items-start">
                <div class="col-lg-8 m-15px-tb">
                    <article class="article">
                        <div class="article-img">
                            <div id="carouselExampleControls" class="carousel slide" data-bs-ride="carousel">
                                <div class="carousel-inner">
                                    <div class="carousel-item" v-for="(image, index) in post.images" :key="index" :class="{ 'active': index === 0 }">
                                        <img :src="image.url" :alt="`Image ${index + 1}`" class="d-block w-100" />
                                    </div>
                                </div>
                                <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleControls" data-bs-slide="prev">
                                    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                                    <span class="visually-hidden">Previous</span>
                                </button>
                                <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleControls" data-bs-slide="next">
                                    <span class="carousel-control-next-icon" aria-hidden="true"></span>
                                    <span class="visually-hidden">Next</span>
                                </button>
                            </div>

                        </div>
                        <div class="article-title">
                            
                            <h2>{{post.title}}</h2>
                           
                            
                            <div class="media">
                                <div class="avatar">
                                    
                                </div>
                                <div class="media-body">
                                    <div class="d-grid gap-2 col-6 mx-auto">
  <button class="btn btn-primary" type="button">Связаться</button>
  
</div>
                                   <!-- <button type="button" class="btn btn-primary btn-lg">Связаться</button>-->
                                </div>
                            </div>
                        </div>
                        <div class="article-content">
                          <h3>Об этом чике</h3>
                          <div class="container">
                            <p >{{ post.body }}</p>
                          </div>
                            
                        </div>
                        
                    </article>
                </div>
                <div class="col-lg-4 m-15px-tb blog-aside">
                    <!-- Author -->
                    <div class="widget widget-author">
                        <div class="widget-title">
                            <h3>Автор</h3>
                        </div>
                        <div class="widget-body">
                            <div class="media align-items-center">
                                <div class="avatar">
                                    <img src="https://bootdey.com/img/Content/avatar/avatar6.png" title="" alt="">
                                </div>
                                <div class="media-body">
                                    <h6 v-if="post.user"> User name {{ post.user.name }} </h6>
                                </div>
                            </div>
                            <!--<p>I design and develop services for customers of all sizes, specializing in creating stylish, modern websites, web services and online stores</p>-->
                        </div>
                    </div>
                    <!-- End Author -->
                    <!-- Trending Post -->
                    
                    <!-- End Trending Post -->
                    <!-- Latest Post -->
                    <div class="widget widget-latest-post">
                        <div class="widget-title">
                            <h3>Рекомендуем также</h3>
                        </div>
                        <div class="widget-body">
    <div v-for="(relatedPost, index) in relatedPosts" :key="index" class="latest-post-aside media">
        <div class="lpa-left media-body">
            <router-link
                :to="{
                    name: 'SingleBlog',
                    params: { slug: relatedPost.slug },
                }"
            >
                <div class="lpa-title">
                    <h5><a href="#">{{ relatedPost.title }}</a></h5>
                </div>
                <div class="lpa-meta">
                    <!--<a class="name" href="#">
                        {{ relatedPost.price }}
                    </a>-->
                </div>
            </router-link>
        </div>
        <div class="lpa-right">
            <img v-if="relatedPost.images.length > 0" :src="relatedPost.images[0].url" :alt="relatedPost.title" />
        </div>
    </div>
</div>



                    </div>
                   
                    <!-- End Latest Post -->
                    <!-- widget Tags -->
                  
                    <!-- End widget Tags -->
                </div>
            </div>
        </div>
    </div>



            <!-- Display post details 
        <section class="single-blog-post">
            <h1>{{ post?.title }}</h1>
            <p class="time-and-author">
                {{ post.created_at }}
                <span v-if="post.user">{{ post.user.name }}</span>
                <span v-else>Unknown</span>
            </p>
           <div v-for="(image, index) in post.images" :key="index">
                    <img :src="image?.url" :alt="`Image ${index + 1}`" style="width: 150px; height: 150px" />
                </div>
            <div class="about-text" v-if="!shouldHideAboutText">
      <p class="price">Цена: {{ post.price }} :Uzs</p>
      <button
        class="buyBtn"
        @click="buyPost(post.id, post.price)"
        :disabled="buying || postIsPurchased"
      >
        Купить
      </button>
      <button
        class="cancelBtn"
        @click="cancelPurchase(post.id, post.price)"
        :disabled="!postIsPurchased"
      >
        Отменить
      </button>
      
    </div>
            <div class="about-text">
                <p>{{ post.body }}</p>
            </div>
        </section>-->

        <!-- Display related posts 
        <section class="recommended">
            <p>Рекомендованные</p>
            <div class="recommended-cards">
                <router-link
                    v-for="relatedPost in relatedPosts"
                    :key="relatedPost.id"
                    :to="{
                        name: 'SingleBlog',
                        params: { slug: relatedPost.slug },
                    }"
                >
                    <div class="recommended-card">
                        <img
                            :src="`/${relatedPost.imagePath}`"
                            alt=""
                            loading="lazy"
                        />
                        <h4>{{ relatedPost.title }}</h4>
                        <h4>Цена: {{ relatedPost.price }}</h4>
                    </div>
                </router-link>
            </div>
        </section>-->

        <!-- Display chat messages -->
        
        </div>
        
    </div>
</template>

<script>
import axios from "axios";


export default {
    props: ["slug"],
    data() {
        return {
            post: {},
            relatedPosts: [],
            
            
            
            error: null,
            currentUser: null,
            cash: "",
            
        };
    },
    methods: {
        
        async fetchData() {
            try {
                const [
                    postResponse,
                    relatedPostsResponse,
                    
                    currentUserResponse,
                ] = await Promise.all([
                    axios.get(`/api/posts/${this.slug}`),
                    axios.get(`/api/related-posts/${this.slug}`),
                    axios.get(`/api/posts/${this.slug}/messages`),
                    axios.get(`/api/user`),
                ]);
                this.post = postResponse.data.data;
                this.relatedPosts = relatedPostsResponse.data.data;
                
                this.currentUser = currentUserResponse.data;
            } catch (error) {
                console.error("Error fetching data:", error);
                this.error = "Error fetching data";
            }
        },
      
    async fetchPostData() {
        try {
            const response = await axios.get("/api/posts/" + this.slug);
            this.post = response.data.data;

            // Check if the authenticated user is the author of the post
            const isCurrentUserPostAuthor = this.post.user.id === Auth.id();

            // Check if user is logged in and is the author of the post
            this.shouldHideAboutText = Auth.isLoggedIn() && isCurrentUserPostAuthor;
        } catch (error) {
            console.log(error);
        }
        },
       
    },
    mounted() {
        this.fetchData();
        
        this.fetchPostData();
    },
};
</script>

<style scoped>
.article .article-content h3 {
  color: black !important;
  font-weight: 400 !important;
}
.lh-base{
  right: 20px;
  
}
.align-items-center{
  display: flex;
}
.media-body button{
    background-color: #8c093d;
}
.buyBtn{
  background-color: orange;
    border-color: white;
    width: 86px;
    border-radius: 40px;
    margin-left: 20px;
    margin-right: 20px;
}
.cancelBtn{
  
  background-color: red;
    border-color: white;
    width: 86px;
    border-radius: 40px;  
    color: white;

}
.price{
  margin-bottom: 0 0;
}
/*post */
.blog-listing {
    padding-top: 30px;
    padding-bottom: 30px;
}
.gray-bg {
    background-color: #f5f5f5;
}
/* Blog 
---------------------*/
.blog-grid {
  box-shadow: 0 0 30px rgba(31, 45, 61, 0.125);
  border-radius: 5px;
  overflow: hidden;
  background: #ffffff;
  margin-top: 15px;
  margin-bottom: 15px;
}
.blog-grid .blog-img {
  position: relative;
}
.blog-grid .blog-img .date {
  position: absolute;
  background: #fc5356;
  color: #ffffff;
  padding: 8px 15px;
  left: 10px;
  top: 10px;
  border-radius: 4px;
}
.blog-grid .blog-img .date span {
  font-size: 22px;
  display: block;
  line-height: 22px;
  font-weight: 700;
}
.blog-grid .blog-img .date label {
  font-size: 14px;
  margin: 0;
}
.blog-grid .blog-info {
  padding: 20px;
}
.blog-grid .blog-info h5 {
  font-size: 22px;
  font-weight: 700;
  margin: 0 0 10px;
}
.blog-grid .blog-info h5 a {
  color: #20247b;
}
.blog-grid .blog-info p {
  margin: 0;
}
.blog-grid .blog-info .btn-bar {
  margin-top: 20px;
}


/* Blog Sidebar
-------------------*/
.blog-aside .widget {
  box-shadow: 0 0 30px rgba(31, 45, 61, 0.125);
  border-radius: 5px;
  overflow: hidden;
  background: #ffffff;
  margin-top: 15px;
  margin-bottom: 15px;
  width: 100%;
  display: inline-block;
  vertical-align: top;
}
.blog-aside .widget-body {
  padding: 15px;
}
.blog-aside .widget-title {
  padding: 15px;
  border-bottom: 1px solid #eee;
}
.blog-aside .widget-title h3 {
  font-size: 20px;
  font-weight: 700;
  color: #fc5356;
  margin: 0;
}
.blog-aside .widget-author .media {
  margin-bottom: 15px;
}
.blog-aside .widget-author p {
  font-size: 16px;
  margin: 0;
}
.blog-aside .widget-author .avatar {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  overflow: hidden;
}
.blog-aside .widget-author h6 {
  font-weight: 600;
  color: #20247b;
  font-size: 22px;
  margin: 0;
  padding-left: 20px;
}
.blog-aside .post-aside {
  margin-bottom: 15px;
}
.blog-aside .post-aside .post-aside-title h5 {
  margin: 0;
}
.blog-aside .post-aside .post-aside-title a {
  font-size: 18px;
  color: #20247b;
  font-weight: 600;
}
.blog-aside .post-aside .post-aside-meta {
  padding-bottom: 10px;
}
.blog-aside .post-aside .post-aside-meta a {
  color: #6F8BA4;
  font-size: 12px;
  text-transform: uppercase;
  display: inline-block;
  margin-right: 10px;
}
.blog-aside .latest-post-aside + .latest-post-aside {
  border-top: 1px solid #eee;
  padding-top: 15px;
  margin-top: 15px;
}
.blog-aside .latest-post-aside .lpa-right {
  width: 90px;
}
.blog-aside .latest-post-aside .lpa-right img {
  border-radius: 3px;
}
.blog-aside .latest-post-aside .lpa-left {
  padding-right: 15px;
}
.blog-aside .latest-post-aside .lpa-title h5 {
  margin: 0;
  font-size: 15px;
}
.blog-aside .latest-post-aside .lpa-title a {
  color: #20247b;
  font-weight: 600;
}
.blog-aside .latest-post-aside .lpa-meta a {
  color: #6F8BA4;
  font-size: 12px;
  text-transform: uppercase;
  display: inline-block;
  margin-right: 10px;
}

.tag-cloud a {
  padding: 4px 15px;
  font-size: 13px;
  color: #ffffff;
  background: #20247b;
  border-radius: 3px;
  margin-right: 4px;
  margin-bottom: 4px;
}
.tag-cloud a:hover {
  background: #fc5356;
}

.blog-single {
  padding-top: 30px;
  padding-bottom: 30px;
}

.article {
  box-shadow: 0 0 30px rgba(31, 45, 61, 0.125);
  border-radius: 5px;
  overflow: hidden;
  background: #ffffff;
  padding: 15px;
  margin: 15px 0 30px;
}
.article .article-title {
  padding: 15px 0 20px;
}
.article .article-title h6 {
  font-size: 14px;
  font-weight: 700;
  margin-bottom: 20px;
}
.article .article-title h6 a {
  text-transform: uppercase;
  color: #fc5356;
  border-bottom: 1px solid #fc5356;
}
.article .article-title h2 {
  color: #20247b;
  font-weight: 600;
}
.article .article-title .media {
  padding-top: 15px;
  border-bottom: 1px dashed #ddd;
  padding-bottom: 20px;
}
.article .article-title .media .avatar {
  width: 45px;
  height: 45px;
  border-radius: 50%;
  overflow: hidden;
}
.article .article-title .media .media-body {
  padding-left: 8px;
}
.article .article-title .media .media-body label {
  font-weight: 600;
  color: #fc5356;
  margin: 0;
}
.article .article-title .media .media-body span {
  display: block;
  font-size: 12px;
}
.article .article-content h1,
.article .article-content h2,
.article .article-content h3,
.article .article-content h4,
.article .article-content h5,
.article .article-content h6 {
  color: #20247b;
  font-weight: 600;
  margin-bottom: 15px;
}
.article .article-content blockquote {
  max-width: 600px;
  padding: 15px 0 30px 0;
  margin: 0;
}
.article .article-content blockquote p {
  font-size: 20px;
  font-weight: 500;
  color: #fc5356;
  margin: 0;
}
.article .article-content blockquote .blockquote-footer {
  color: #20247b;
  font-size: 16px;
}
.article .article-content blockquote .blockquote-footer cite {
  font-weight: 600;
}
.article .tag-cloud {
  padding-top: 10px;
}

.article-comment {
  box-shadow: 0 0 30px rgba(31, 45, 61, 0.125);
  border-radius: 5px;
  overflow: hidden;
  background: #ffffff;
  padding: 20px;
}
.article-comment h4 {
  color: #20247b;
  font-weight: 700;
  margin-bottom: 25px;
  font-size: 22px;
}
img {
    max-width: 100%;
}
img {
    vertical-align: middle;
    border-style: none;
}

/* Contact Us
---------------------*/
.contact-name {
  margin-bottom: 30px;
}
.contact-name h5 {
  font-size: 22px;
  color: #20247b;
  margin-bottom: 5px;
  font-weight: 600;
}
.contact-name p {
  font-size: 18px;
  margin: 0;
}

.social-share a {
  width: 40px;
  height: 40px;
  line-height: 40px;
  border-radius: 50%;
  color: #ffffff;
  text-align: center;
  margin-right: 10px;
}
.social-share .dribbble {
  box-shadow: 0 8px 30px -4px rgba(234, 76, 137, 0.5);
  background-color: #ea4c89;
}
.social-share .behance {
  box-shadow: 0 8px 30px -4px rgba(0, 103, 255, 0.5);
  background-color: #0067ff;
}
.social-share .linkedin {
  box-shadow: 0 8px 30px -4px rgba(1, 119, 172, 0.5);
  background-color: #0177ac;
}

.contact-form .form-control {
  border: none;
  border-bottom: 1px solid #20247b;
  background: transparent;
  border-radius: 0;
  padding-left: 0;
  box-shadow: none !important;
}
.contact-form .form-control:focus {
  border-bottom: 1px solid #fc5356;
}
.contact-form .form-control.invalid {
  border-bottom: 1px solid #ff0000;
}
.contact-form .send {
  margin-top: 20px;
}
@media (max-width: 767px) {
  .contact-form .send {
    margin-bottom: 20px;
  }
}

.section-title h2 {
    font-weight: 700;
    color: #20247b;
    font-size: 45px;
    margin: 0 0 15px;
    border-left: 5px solid #fc5356;
    padding-left: 15px;
}
.section-title {
    padding-bottom: 45px;
}
.contact-form .send {
    margin-top: 20px;
}
.px-btn {
    padding: 0 50px 0 20px;
    line-height: 60px;
    position: relative;
    display: inline-block;
    color: #20247b;
    background: none;
    border: none;
}
.px-btn:before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    display: block;
    border-radius: 30px;
    background: transparent;
    border: 1px solid rgba(252, 83, 86, 0.6);
    border-right: 1px solid transparent;
    -moz-transition: ease all 0.35s;
    -o-transition: ease all 0.35s;
    -webkit-transition: ease all 0.35s;
    transition: ease all 0.35s;
    width: 60px;
    height: 60px;
}
.px-btn .arrow {
    width: 13px;
    height: 2px;
    background: currentColor;
    display: inline-block;
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto;
    right: 25px;
}
.px-btn .arrow:after {
    width: 8px;
    height: 8px;
    border-right: 2px solid currentColor;
    border-top: 2px solid currentColor;
    content: "";
    position: absolute;
    top: -3px;
    right: 0;
    display: inline-block;
    -moz-transform: rotate(45deg);
    -o-transform: rotate(45deg);
    -ms-transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
    transform: rotate(45deg);
}

/* Your existing styles */
</style>
