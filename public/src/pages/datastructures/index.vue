<template>
  <div>
    <SectionHeader/>
    <main class="main">
      <div class="container mt-5 py-5">
        <div class="row m-5 p-5">
          <div clas="col-12">
            <h1 class="text-center pb-5">Data Structures Visualization</h1>
          </div>
          <div class="col-4 mb-5">
            <div class="card">
              <img class="visual-image" src="/src/assets/img/gif/arrays.gif" />
              <div class="card-body">
                <h5>Arrays Visualization</h5>
                <p><small>Show indexed collection of elements in a sequential layout.</small></p>
                <button class="btn btn-primary w-100" @click="()=>{ $router.push('/datastructures-array'); }" >
                  <i class="bi bi-unlock-fill me-1"></i>
                  <span>View</span>
                </button>
              </div>
            </div>
          </div>
          <div class="col-4 mb-5">
            <div class="card">
              <img class="visual-image" src="/src/assets/img/gif/linkedlist.gif" />
              <div class="card-body">
                <h5>Linked List Visualization</h5>
                <p><small>Display nodes linked sequentially with pointers or references.</small></p>
                <button class="btn btn-primary w-100" :disabled="!views?.ARRAY" @click="()=>{ $router.push('/datastructures-linked-list'); }">
                  <i v-if="!views?.ARRAY" class="bi bi-lock-fill me-1"></i>
                  <i v-else class="bi bi-unlock-fill me-1"></i>
                  <span>View</span>
                </button>
              </div>
            </div>
          </div>
          <div class="col-4 mb-5">
            <div class="card">
              <img class="visual-image" src="/src/assets/img/gif/graphs.gif" />
              <div class="card-body">
                <h5>Graphs Visualization</h5>
                <p><small>Visualize nodes and edges with directed or undirected connections</small></p>
                <button class="btn btn-primary w-100" :disabled="!views?.LINKED_LIST" @click="()=>{ $router.push('/datastructures-graphs'); }">
                  <i v-if="!views?.LINKED_LIST" class="bi bi-lock-fill me-1"></i>
                  <i v-else class="bi bi-unlock-fill me-1"></i>
                  <span>View</span>
                </button>
              </div>
            </div>
          </div>
          <div class="col-4 mb-5">
            <div class="card">
              <img class="visual-image" src="/src/assets/img/gif/stacks.gif" />
              <div class="card-body">
                <h5>Stacks Visualization</h5>
                <p><small>Represent LIFO structure with push, pop, and peek actions.</small></p>
                <button class="btn btn-primary w-100" :disabled="!views?.GRAPHS" @click="()=>{ $router.push('/datastructures-stacks'); }">
                  <i v-if="!views?.STACKS" class="bi bi-lock-fill me-1"></i>
                  <i v-else class="bi bi-unlock-fill me-1"></i>
                  <span>View</span>
                </button>
              </div>
            </div>
          </div>
          <div class="col-4 mb-5">
            <div class="card">
              <img class="visual-image" src="/src/assets/img/gif/queues.gif" />
              <div class="card-body">
                <h5>Queues Visualization</h5>
                <p><small>Simulate FIFO structure with enqueue and dequeue operations.</small></p>
                <button class="btn btn-primary w-100" :disabled="!views?.STACKS" @click="()=>{ $router.push('/datastructures-queues'); }">
                  <i v-if="!views?.QUEUES" class="bi bi-lock-fill me-1"></i>
                  <i v-else class="bi bi-unlock-fill me-1"></i>
                  <span>View</span>
                </button>
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
  import { lsGetUser, printDevLog, queryURL } from '@/uikit-api';
  import SectionHeader from "@/components/SectionHeader.vue";
  import SectionFooter from "@/components/SectionFooter.vue";

  export default defineComponent({
    name: "CoursesPage",
    components: { SectionFooter, SectionHeader },
    data() {
      return {
        user: {} as any,
        views: {} as any
      }
    },
    methods: {
      async onFetchViews() {
        await queryURL({ url: "util_quiz/visualViewsChecker?user_refid=" + this.user?.user_refid }).then( async (response) => {
          this.views = response;
        });
      }
    },
    async mounted() {
      const user = await lsGetUser() as any;
      if(user?.user_refid) {
        this.user = user;
      }
      await this.onFetchViews().then( async () => {
        printDevLog("DS Data:", toRaw(this.$data));
      });
    },
  });

</script>
<style scoped>
  .visual-image {
    width: 100%;
    height: 280px;
    object-fit: contain;
    object-position: center;
  }
</style>