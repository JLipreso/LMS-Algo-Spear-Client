<template>
  <div>
    <SectionHeader/>
    <main class="main">
      <div class="container mt-5 py-5">
        <h1 class="text-center mt-5">Data Structure</h1>
        <div class="row mt-5">
          <div class="col-4">
            <CourseList title="Data Structure" :list="list" @view="onView" />
          </div>
          <div class="col-8">            
            <h3 class="text-center my-4">{{ article?.description }}</h3>
            <div class="text-dark">
              <div v-if="article?.content" v-html="article?.content"></div>
              <div v-else class="p-5 m-5">
                <div class="card">
                  <div class="card-body">
                    <h3 class="text-center p-5" style="color: #a9a0a0;">Select reading material to start</h3>
                    <div class="d-flex justify-content-center pb-5">
                      <svg xmlns="http://www.w3.org/2000/svg" width="80" height="80" fill="#a9a0a0" class="bi bi-bookmark-plus-fill" viewBox="0 0 16 16">
                        <path fill-rule="evenodd" d="M2 15.5V2a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v13.5a.5.5 0 0 1-.74.439L8 13.069l-5.26 2.87A.5.5 0 0 1 2 15.5m6.5-11a.5.5 0 0 0-1 0V6H6a.5.5 0 0 0 0 1h1.5v1.5a.5.5 0 0 0 1 0V7H10a.5.5 0 0 0 0-1H8.5z"/>
                      </svg>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
    <SectionFooter/>
  </div>
</template>
<script lang="ts">

  import { defineComponent, toRaw } from 'vue';
  import { lsGetUser, fetchAllArticlesFunOfProg, printDevLog, fetchSingleArticleByTopic, scrollToTop, createReadLogs } from "@/uikit-api";
  import SectionHeader from "@/components/SectionHeader.vue";
  import SectionFooter from "@/components/SectionFooter.vue";
  import CourseList from "@/components/Courses.vue";

  export default defineComponent({
    components: { CourseList, SectionFooter, SectionHeader },
    data() {
      return {
        user: {} as any,
        list: [] as any,
        article: {} as any
      }
    },
    methods: {
      async onView(event: any) {
        await fetchSingleArticleByTopic(event?.topic_refid).then( async (article) => {
          if(article.length > 0) {
            scrollToTop();
            this.article = article[0];
            await createReadLogs({ article_refid: article[0]?.article_refid, topic_refid: article[0]?.topic_refid, user_refid: this.user?.user_refid});
          }
          else {
            this.$toast.warning("No article found");
          }
        });
      }
    },
    async mounted() {
      const user = await lsGetUser() as any;
      if(user?.user_refid) {
        this.user = user;
      }
      else {
        this.$toast.warning("Login to log reading history.")
      }
      await fetchAllArticlesFunOfProg().then( async (list) => {
        this.list = toRaw(list);
        printDevLog("Data:", this.$data);
      });
    },
  });

</script>