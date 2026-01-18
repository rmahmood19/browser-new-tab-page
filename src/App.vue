<script setup>
import {computed, ref} from "vue";
import TabButton from "./components/TabButton.vue";

const currentTab = ref('work')

const savedBookmarks = JSON.parse(localStorage.getItem('bookmarks') || '[]')

const bookmarks = ref(savedBookmarks);

const handleTabChange = (e) => {
    currentTab.value = e;
}

const handleSubmit = () => {
    bookmarks.value.push({...formData.value})
    localStorage.setItem('bookmarks', JSON.stringify(bookmarks.value));
    formData.value = {
        name: '',
        url: '',
        category: '',
    };

}

const categories = [
    {label: 'Work', value: 'work'},
    {label: 'Personal', value: 'personal'},
    {label: 'Study', value: 'study'},
    {label: 'Misc', value: 'misc'}
];

const formData = ref({
    'name': '',
    'url': '',
    'category': '',
    'icon': '',
})

const filteredBookmarks = computed(() =>
    bookmarks.value.filter( b => b.category === currentTab.value )
)
</script>

<template>

    <div class="bg-orange-100 h-screen">
        <div class="flex justify-end p-6 ">
            <div class="dropdown dropdown-end">
                <div tabindex="0" role="button" class="btn border-2 btn-outline btn-accent rounded-full">Add New bookmark
                </div>
                <div tabindex="-1"
                     class=" flex dropdown-content menu bg-slate-900 rounded-xl z-1 w-98 h-96 p-2 mt-4 shadow-sm">
                    <div class="">
                        <form @submit.prevent="handleSubmit" class="flex flex-col bg-slate-900 justify-center">
                            <div class="p-4">
                                <input v-model="formData.name" type="text" placeholder="Name"
                                       class="input rounded-xl w-full input-accent mt-4 outline-0"/>
                                <input v-model="formData.url" type="text" placeholder="Url"
                                       class="input rounded-xl w-full input-accent mt-4 outline-0"/>
                                <input v-model="formData.icon" type="text" placeholder="Name of icon form tabler icons"
                                       class="input rounded-xl w-full input-accent mt-4 outline-0"/>
                                <select v-model="formData.category"
                                        class="select rounded-xl select-accent mt-4 w-full outline-0">
                                    <option disabled selected>Select a category</option>
                                    <option v-for="category in categories" :value="category.value">
                                        {{ category.label }}
                                    </option>
                                </select>
                                <div class="flex justify-center">
                                    <button type="submit" class="btn btn-accent mx-auto mt-4 px-12 rounded-full">Save</button>
                                </div>
                            </div>

                        </form>
                    </div>
                </div>
            </div>
        </div>
        <div class="max-w-lg mx-auto mt-12">
            <div class="relative rounded-full p-px">
                <!-- glow -->
                <div class="absolute inset-0 rounded-full
              bg-linear-to-r from-teal-800 via-red-500 to-sky-800
              blur-md "></div>

                <!-- content -->
                <div class="relative rounded-full bg-slate-900 backdrop-blur p-2">
                    <div class="flex justify-around">
                        <TabButton
                            v-for="category in categories"
                            :key="category.value"
                            :label="category.label"
                            :value="category.value"
                            :active="currentTab === category.value"
                            @select="handleTabChange"
                        />
                    </div>
                </div>

            </div>
        </div>
        <div class="flex gap-4 justify-center pt-16 ">
            <div v-for="bookmark in filteredBookmarks" class="">
                <a :href="bookmark.url">
                    <div class="w-48 p-2 flex flex-col
            justify-items-center text-center glass
          transition-shadow duration-300
          hover:shadow-[0_0_25px_rgba(56,189,248,0.6)] ">
                        <div class="flex justify-center text-red-400">

                            <div class="">
                                <i class="fa-solid fa-2xl" :class="bookmark.icon"></i>
                            </div>

<!--                            <component :is="icons[bookmark.icon]" size="48" stroke="2"></component>-->
                        </div>

                        <div class="text-center font-bold text-sky-500 text-lg pt-2">{{ bookmark.name }}</div>

                    </div>
                </a>
            </div>
        </div>
    </div>


</template>

<style scoped>

.bg-gray-800 {
    border: 2px solid transparent;
    transition: border-color 0.8s, opacity 0.8s;
    opacity: 0.7;
}

.border-fade-in {
    border-color: oklch(70.4% 0.14 182.503); /* Tailwind's red-200 */
    opacity: 1;
}

.border-2 {
    opacity: 1;
}

.glass {
    background: rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px); /* Safari */

    border-left: 8px solid oklch(70.4% 0.14 182.503);

    border-radius: 16px;

    padding: 12px;
    color: #fff;
}


</style>
