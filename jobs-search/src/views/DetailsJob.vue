<script setup>
  import {ref, onMounted} from 'vue'
  import PulseLoader from 'vue-spinner/src/PulseLoader.vue'
  import { useJobsStore } from '@/stores/jobs';
  import { useRoute } from 'vue-router';
  import { RouterLink } from 'vue-router';
  import BackButton from '@/components/BackButton.vue';

  const route = useRoute()
  const jobStore = useJobsStore()
  let id = null

  onMounted(() =>{
    id = route.params.id
    jobStore.fetchOneJob(id)
    // console.log(router);
    // console.log(route, 'route');
  })

  async function deleteJob(){
    await jobStore.deleteJob(id);
  }
</script>
<template>
      <BackButton />
     <section v-if="!jobStore.isLoading" class="bg-green-50">
      <div class="container m-auto py-10 px-6">
        <div class="grid grid-cols-1 md:grid-cols-70/30 w-full gap-6">
          <main>
            <div
              class="bg-white p-6 rounded-lg shadow-md text-center md:text-left"
            >
              <div class="text-gray-500 mb-4">{{ jobStore.job?.type }}</div>
              <h1 class="text-3xl font-bold mb-4">{{ jobStore.job?.title }}</h1>
              <div
                class="text-gray-500 mb-4 flex align-middle justify-center md:justify-start"
              >
              <span class="material-symbols-outlined text-orange-700">
                location_on
              </span>
                <p class="text-orange-700">{{ jobStore.job?.location }}</p>
              </div>
            </div>

            <div class="bg-white p-6 rounded-lg shadow-md mt-6">
              <h3 class="text-green-800 text-lg font-bold mb-6">
                Job Description
              </h3>

              <p class="mb-4">
                {{ jobStore.job?.description }}
              </p>

              <h3 class="text-green-800 text-lg font-bold mb-2">Salary</h3>

              <p class="mb-4">{{ jobStore.job?.salary }} / Year</p>
            </div>
          </main>

          <!-- Sidebar -->
          <aside>
            <!-- Company Info -->
            <div class="bg-white p-6 rounded-lg shadow-md">
              <h3 class="text-xl font-bold mb-6">Company Info</h3>

              <h2 class="text-2xl">{{ jobStore.job?.company?.name }}</h2>

              <p class="my-2">
                {{ jobStore.job?.company?.description }}
              </p>

              <hr class="my-4" />

              <h3 class="text-xl">Contact Email:</h3>

              <p class="my-2 bg-green-100 p-2 font-bold">
                {{ jobStore.job?.company?.contactEmail }}
              </p>

              <h3 class="text-xl">Contact Phone:</h3>

              <p class="my-2 bg-green-100 p-2 font-bold">{{ jobStore.job?.company?.contactPhone }}</p>
            </div>

            <!-- Manage -->
            <div class="bg-white p-6 rounded-lg shadow-md mt-6">
              <h3 class="text-xl font-bold mb-6">Manage Job</h3>
              <RouterLink
                :to="'/jobs/edit/' + jobStore.job?.id"
                class="bg-green-500 hover:bg-green-600 text-white text-center font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
                >Edit Job</RouterLink
              >
              <button
                @click="deleteJob"
                class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block"
              >
                Delete Job
              </button>
            </div>
          </aside>
        </div>
      </div>
    </section>
    <div 
        v-else
        class="text-center text-gray-500 py-6"
    >
        <PulseLoader />
    </div>

</template>