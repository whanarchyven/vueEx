<template>
  <div>
    <h1>Объявления на Brom.ru</h1>
    <!--    <my-input v-model="searchQuery" placeholder="Поиск по названию"></my-input>-->
    <!--    <div class="app_btns">-->
    <!--      <my-button @click="showDialog">Создать пост</my-button>-->
    <!--      <my-select v-model="selectedSort" :options="sortOptions"></my-select>-->
    <!--    </div>-->
    <!--    <modal-window v-model:show="dialogVisible"><h2>Создание поста</h2>-->
    <!--      <post-form @create="createPost"/>-->
    <!--    </modal-window>-->
    <my-button @click="this.dialogVisible = true"
      >Опубликовать объявление</my-button
    >
    <!--    <my-button @click="this.createApart">Опубликовать квартиру</my-button>-->
    <modal-window v-model:show="dialogVisible">
      <car-form @create="sendCar"></car-form>
    </modal-window>
    <post-list
      :posts="this.$store.state.posts"
      @remove="deletePost"
      v-if="!isPostLoading"
    />
    <div v-else>
      <loading
        v-model:active="this.isPostLoading"
        :can-cancel="false"
        :is-full-page="true"
        :color="'#008000'"
      />
    </div>
    <!--    <div class="page__wrapper">-->
    <!--      <div-->
    <!--          v-for="pageNumber in totalPage"-->
    <!--          :key="page"-->
    <!--          class="page"-->
    <!--          :class="{-->
    <!--            'current-page':page===pageNumber?true:false-->
    <!--          }"-->
    <!--          @click="changePage(pageNumber)"-->
    <!--      >-->
    <!--        {{pageNumber}}-->
    <!--      </div>-->
    <!--    </div>-->
    <!--    <div v-intersection="loadMorePosts" class="observer"></div>-->
  </div>
</template>

<script>
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css";
// import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
// import ModalWindow from "@/components/UI/ModalWindow";
// import MyButton from "@/components/UI/MyButton";
import axios from "axios";
// import MySelect from "@/components/UI/MySelect";
//import MyInput from "@/components/UI/MyInput";
import { mapActions } from "vuex";
import MyButton from "../components/UI/MyButton";
import ModalWindow from "../components/UI/ModalWindow";
import CarForm from "./Exam/CarForm";

export default {
  components: {
    CarForm,
    ModalWindow,
    MyButton,
    //MyInput,
    PostList,
    Loading,
  },
  data() {
    return {
      posts: [],
      objToCreate: {},
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: "",
      searchQuery: "",
      page: 0,
      limit: 10,
      totalPage: 0,
      sortOptions: [
        { value: "title", name: "По названию" },
        { value: "body", name: "По описанию" },
      ],
    };
  },
  methods: {
    createCar() {
      this.objToCreate = {
        type: "car",
        city: "",
        address: "",
        phone: "",
        price: "",
        model: "",
        car_type: "",
        engine_volume: "",
        engine_power: "",
      };
      this.dialogVisible = true;
    },
    sendCar(item) {
      axios.post(
        "https://demo-api.vsdev.space/api/brom/sales",
        item
      );
      // this.new_commentary.comment='';
      // this.new_commentary.user_name='';
      this.dialogVisible = false;
      this.objToCreate = {};
      this.fetchPosts();
    },
    createApart() {
      this.objToCreate = {
        type: "apartment",
        city: "",
        address: "",
        phone: "",
        price: "",
        rooms: "",
        square: "",
      };
      this.dialogVisible = true;
    },
    // sendApart() {
    //   axios.post(
    //     "https://demo-api.vsdev.space/api/brom/sales",
    //     this.objToCreate
    //   );
    //   // this.new_commentary.comment='';
    //   // this.new_commentary.user_name='';
    //   this.dialogVisible = false;
    //   this.objToCreate = {};
    //   this.fetchPosts();
    // },
    // createPost(post) {
    //   this.posts.push(post);
    //   this.dialogVisible = false;
    // },
    // deletePost(post) {
    //   this.posts.forEach(function (item, i) {
    //     if (item.id == post.id) {
    //       post.id = i;
    //     }
    //   });
    //   this.posts.splice(post.id, 1);
    // },

    showDialog() {
      this.dialogVisible = true;
    },
    // async fetchPosts() {
    //   try {
    //     this.isPostLoading = true;
    //     setTimeout(async () => {
    //       const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
    //         params: {
    //           _page: this.page,
    //           _limit: this.limit,
    //         }
    //       });
    //       this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit);
    //       this.posts = response.data;
    //       this.isPostLoading = false;
    //       this.page += 1;
    //     }, 600)
    //
    //   } catch (error) {
    //     alert('ERROR');
    //   }
    //
    // },
    ...mapActions(["fetchPosts"]),
    // async loadMorePosts() {
    //   try {
    //     const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
    //       params: {
    //         _page: this.page,
    //         _limit: this.limit,
    //       }
    //     });
    //     this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit);
    //     this.posts = [...this.posts, ...response.data];
    //     this.isPostLoading = false;
    //     this.page += 1;
    //   } catch (error) {
    //     alert('ERROR');
    //   }
    // },
    // changePage(pageNumber){
    //   this.page=pageNumber;
    //   // this.fetchPosts();
    // }
  },
  mounted() {
    //console.log(this.$refs.observer);
    // const options = {
    //   rootMargin: '0px',
    //   threshold: 1.0
    // }
    // const callback = (entries)=> {
    //   if(entries[0].isIntersecting && this.page<this.totalPage){
    //     this.page+=1;
    //     this.loadMorePosts();
    //   }
    // };
    // const observer = new IntersectionObserver(callback, options);
    // observer.observe(this.$refs.observer);
  },
  computed: {
    // sortedPost() {
    //   return [...this.$store.state.posts].sort((post1, post2) => {
    //     return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
    //   })
    // },
    // sortedAndSearchedPost() {
    //   return this.sortedPost.filter(post => post.name.toLowerCase().includes(this.searchQuery.toLowerCase()))
    // },
  },
  watch: {
    updateVisible(){
      this.fetchPosts();
    }
    // selectedSort(newValue){
    //   this.posts.sort((post1,post2)=>{
    //     return post1[newValue]?.localeCompare(post2[newValue])
    //   })
    // },
    // page(){
    //   this.fetchPosts();
    // }
  },
  beforeMount() {
    this.fetchPosts();
  },
};
</script>

<style>
.app_btns {
  margin: 10px;
  display: flex;
  justify-content: space-between;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid green;
  background: transparent;
  color: black;
  padding: 10px;
}

.current-page {
  background: green;
  color: white;
}

.observer {
  height: 30px;
  background: transparent;
}
</style>
