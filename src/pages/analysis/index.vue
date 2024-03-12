<template>
    <div>
        <div class="h-screen flex flex-col pb-6">
            <div class="h-full flex flex-col justify-center">
                <div class="-mt-20 max-w-4xl w-full text-center mx-auto px-4 sm:px-6 lg:px-8">

                    <h1 class="text-3xl font-bold text-gray-800 sm:text-4xl dark:text-white">
                        Insights Engine
                    </h1>
                    <p class="mt-3 text-sm text-gray-500 dark:text-gray-400">
                        Stay in the know with insights from industry experts.
                    </p>
                </div>

                <div class="mt-10 w-full max-w-lg mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="relative">
                        <input type="text" v-model="query" @keydown.enter.prevent="search" autofocus
                            class="p-4 block w-full border-dark bg-gray-100 outline-dark rounded-full text-sm focus:border-blue-500 focus:ring-blue-500 disabled:opacity-50 disabled:pointer-events-none dark:bg-slate-900 dark:border-gray-700 dark:text-gray-400 dark:focus:ring-gray-600"
                            placeholder="what's on your mind?">
                        <div class="absolute top-1/2 end-2 -translate-y-1/2">
                            <button type="button" @click="search"
                                class="size-10 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-500 hover:text-gray-800 bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-gray-400 dark:hover:text-white dark:bg-gray-800 dark:focus:outline-none dark:focus:ring-1 dark:focus:ring-gray-600">
                                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor"
                                    class="bi bi-arrow-right-circle-fill" viewBox="0 0 16 16">
                                    <path
                                        d="M8 0a8 8 0 1 1 0 16A8 8 0 0 1 8 0M4.5 7.5a.5.5 0 0 0 0 1h5.793l-2.147 2.146a.5.5 0 0 0 .708.708l3-3a.5.5 0 0 0 0-.708l-3-3a.5.5 0 1 0-.708.708L10.293 7.5z" />
                                </svg>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- <div class="relative font-mono py-4 px-3 bg-light mt-5 rounded-md">
                    {{ results }}
                </div> -->
            </div>

            <div class="max-w-[85rem] px-4 py-10 sm:px-6 lg:px-8 lg:py-14 mx-auto" v-if="query && results.length > 0">
                <div class="flex flex-col">
                    <div class="-m-1.5 overflow-x-auto">
                        <div class="p-1.5 min-w-full inline-block align-middle">
                            <div
                                class="bg-white border border-gray-200 rounded-xl shadow-sm overflow-hidden dark:bg-slate-900 dark:border-gray-700">
                                <table class="min-w-full divide-y divide-gray-200 dark:divide-gray-700 rounded-lg">
                                    <thead class="bg-gray-50 dark:bg-slate-800 rounded-tl-lg rounded-tr-lg">
                                    </thead>
                                    <tbody class="divide-y divide-gray-200 dark:divide-gray-700">
                                        <tr class="bg-white hover:bg-gray-50 dark:bg-slate-900 dark:hover:bg-slate-800 rounded-lg"
                                            v-for="result in results" :key="result.id">
                                            <td class="px-6 py-4 flex items-center gap-x-4 whitespace-nowrap align-top">
                                                <img :src="getImageUrl(result.url)" loading="eager" decoding="sync"
                                                    alt="favicon" class="h-8 w-8 rounded-full">
                                                <a class="flex flex-col" :href="result.url">
                                                    <span
                                                        class="text-sm font-semibold text-gray-800 dark:text-gray-200">{{
                                                        result.title }}</span>
                                                    <span class="text-xs text-blue-800">{{ result.url }}</span>
                                                </a>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>


            <footer class="mt-auto max-w-4xl text-center mx-auto px-4 sm:px-6 lg:px-8">
                <p class="text-xs text-gray-600 dark:text-gray-500">© 2024 Insights Engine. A product of <a
                        class="text-slate-900 decoration-2 hover:underline font-semibold dark:text-gray-300"
                        href="https://leaphustle.com/" target="_blank">Leap Hustle</a>.</p>
            </footer>
        </div>
    </div>
</template>

<script setup lang="ts">
import ky from 'ky';
import { ref } from 'vue';

const query = ref('');
const results = ref([]);

const getImageUrl = (url: string) => {
    return `https://www.google.com/s2/favicons?domain=${url}&sz=64`;
}

async function search() {
    const endpoint = 'https://insights-engine.lulz.workers.dev/api/search?query=';
    const api = endpoint + query.value;
    const { response } = await ky(api).json();
    results.value = response.results;
}

</script>