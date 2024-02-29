<script>
export default {
    data() {
        return {
            selectedTab: 'all',
        };
    },
    methods: {
        //  切換頁面
        changeTab(tab) {
            this.selectedTab = tab;
        },
    },
};
</script>

<template>
    helllllo
    <div class="bg-blue-100 w-screen h-screen flex flex-col p-6">
        <div class="add w-[400px] h-[50px]">
            <input type="text" placeholder="填寫新增事項" class=" h-[30px] w-[200px] rounded-md me-5">
            <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md">新增</button>
        </div>
        <div class="btn-all w-11/12 h-[45px] flex border-b-2 border-slate-500 gap-5">
            <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md" :class="{ 'active': selectedTab === 'all' }" @click="changeTab('all')">全部</button>
            <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md" :class="{ 'active': selectedTab === 'done' }" @click="changeTab('done')">已執行</button>
            <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md" :class="{ 'active': selectedTab === 'undone' }" @click="changeTab('undone')">未執行</button>
        </div>
        <table class="todo w-11/12 border-b-2 border-slate-500">
            <thead>
                <tr class="flex place-content-around">
                    <td class="font-bold">執行</td>
                    <td class="font-bold">事項</td>
                    <td class="font-bold">功能</td>
                </tr>
            </thead>
            <tbody class="data-show w-11/12">
                <tr class="flex place-content-around">
                    <td><input type="checkbox"></td>
                    <td>第一筆</td>
                    <td>
                        <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md me-2">編輯</button>
                        <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md">刪除</button>
                    </td>
                </tr>
            </tbody>

        </table>
    </div>
</template>


<style>
.active {
    @apply bg-white text-black;
}
</style>