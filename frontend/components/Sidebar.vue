<script setup lang="ts">
import { usePagesStore } from '@/store/pagesStore';
import { useUiStore } from '@/store/uiStore';
import { useRouter } from "vue-router";

const router = useRouter();
const pagesStore = usePagesStore();
const uiStore = useUiStore();


const addPage = async () => {
  try {
    const newPageId = await pagesStore.addPage("New Page", "Text");
    router.push(`/${pagesStore.currentPage._id}`);
  } catch (error: any) {
    console.error("Failed to add page:", error.message);
  }
};

const deletePage = async (id: string) => {
  try {
    await pagesStore.deletePage(id);
  } catch (error: any) {
    console.error("Failed to delete page:", error.message);
  }
};
</script>

<template>
<transition name="slide">
      <div v-if="uiStore.isSidebarOpen" class="sideBar-container">
        <h3 class="main-page__link">
          <router-link :to="`/`">Welcome Page</router-link>
        </h3>
        <ul class="list" v-if="pagesStore.list.length > 0">
          <li class="list-item" v-for="(page, index) in pagesStore.list" :key="page._id">
            <router-link :to="`/${page._id}`">{{ page.title }}</router-link>
            <button @click="deletePage(page._id)">delete</button>
          </li>
        </ul>
        <div v-else>Hmmm... Pages...</div>
        <button @click="addPage">Add Page</button>
      </div>
    </transition>
</template>


<style scoped>
.sideBar-container {
  min-width: 250px;
  max-width: 15%;
  height: 100vh;
  background: #262323;
  transition: transform 0.3s ease;
  overflow: hidden;
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.3s ease;
}

.slide-enter-from,
.slide-leave-to {
  transform: translateX(-100%);
}

.main-page__link a{
  text-decoration: none;
  color: #b2b2b1;
}

.main-page__link a{
  text-decoration: none;
  color: #b2b2b1;
}

.list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.list-item {
  margin: 10px 0;
  font-size: 18px;
  display: flex;
  justify-content: space-between;
  align-content: center;
}

.list-item a {
  text-decoration: none;
  color: #b2b2b1;
  transition: color 0.3s ease;
}

.list-item a:hover {
  color: #ffffff;
}

.list-item button {
  border: none;
  outline: none;
  background: none;
  box-shadow: none;
  font-size: 12px;
  color: blanchedalmond;
  background-color: inherit;
  border-radius: 12px;
}

.list-item button:hover {
  background-color: gray;
  border: 0.5px solid blanchedalmond;
}
</style>
