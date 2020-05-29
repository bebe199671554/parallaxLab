<template>
  <q-page class="flex flex-start">
    <section class="content">
      <section class="parallax-block parallax bg-img">
        <div class="parallax-img">
          <section class=" relative-position github-repo q-pa-md row items-start bg-white">
            <section class="user-info-block  row items-end q-gutter-x-md">
              <q-avatar rounded size="250px">
                <q-img :src="myInfo.avatar_url" native-context-menu>
                  <div class="absolute-bottom text-subtitle1 text-center">{{myInfo.name}}</div>
                </q-img>
              </q-avatar>
              <div class="info-text q-gutter-y-md">
                <div class="text-h4">{{myInfo.login}}</div>
        <div class="text-h6 text-grey">{{myInfo.bio}}</div>

                <div class="row">
          <q-btn flat round color="primary" icon="home" size="lg" type="a" :href="myInfo.html_url" />

                </div>
                
              </div>
            </section>
            <GitHubItem class v-for="(item, index) in myRepoList" :key="index" :repoItem="item" />
          </section>
        </div>
      </section>
    </section>
  </q-page>
</template>

<script>
import { mapState, mapActions } from "vuex";
import GitHubItem from "../components/GitHubItem";
import user from "../store/modules/gitHubAPI/user";
export default {
  name: "PageIndex",
  data() {
    return {
      myRepoList: [],
      myInfo: []
    };
  },
  components: { GitHubItem },
  computed: {
    ...mapState({
      gitHubUserRpeoList: state => state.gitHubAPI.repos.gitHubUserRpeoList,
      gitHubUserInfoList: state => state.gitHubAPI.user.gitHubUserInfoList
    })
  },
  methods: {
    ...mapActions({
      gitHubUserRpeo: "gitHubAPI/repos/gitHubUserRpeo",
      gitHubUserInfo: "gitHubAPI/user/gitHubUserInfo"
    }),

    getUserRpeoList() {
      this.gitHubUserRpeo().then(res => {
        this.myRepoList = this.gitHubUserRpeoList;
      });
    },
    getUserInfo() {
      this.gitHubUserInfo().then(res => {
        console.log(this.gitHubUserInfoList);
        this.myInfo = this.gitHubUserInfoList;
      });
    }
  },
  beforeMount() {
    this.getUserRpeoList();
    this.getUserInfo();
  }
};
</script>

<style lang="scss" scoped>
.user-info-block {
 margin: 0 auto;
  @media (min-width: 572px) {
        position: absolute;
  top: 0;
  transform: translateY(-30%);
    }

}
.github-repo {
  @media (min-width: 572px) {
  padding-top: 200px;
  }
}
.content {
  height: 100vh;
  overflow: hidden;
}
.content::after {
}
.parallax-block {
  transform-style: preserve-3d;
  position: relative;
  perspective: 1px;
  overflow-x: hidden;
  overflow-y: scroll;
}

.bg-img::after {
  background: url("https://cdn.quasar.dev/img/parallax2.jpg");
}
.parallax-img::after {
  height: 70vh;
  width: 100%;
  background-color: aliceblue;
}
.parallax-img {
  box-sizing: border-box;
  min-height: 40vw;
  padding: 30vw 0 5vw;
  position: relative;

  transform-style: inherit;
  width: 100vw;
}
.parallax-img,
.parallax-img:before {
  background: 50% 50% / cover;
}
.parallax-img::before {
  content: "";
  left: 0;
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  display: block;
  background-image: url(https://cdn.quasar.dev/img/parallax2.jpg);
  background-size: cover;
  transform-origin: center center 0;
  transform: translateZ(-1px) scale(2);
  z-index: -1;
  min-height: 100vh;
}

.parallax-img p {
  background-color: hsla(0, 0%, 100%, 0.1);
}
</style>
