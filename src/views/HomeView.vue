<script>
import { pushScopeId } from 'vue';

export default {
    data() {
        return {
            selectedTab: 'all',
            arr: [],
            obj: {},
            msg: '',
        };
    },
    // mounted要寫在外面！
    mounted() {
        // 當網頁打開時，先執行裡面的JS
        // 屬於created的部分
        // 先把資料從localStorage特定key拿出資料，丟入arr裡面
        // localStorage.removeItem('msg');
        if (localStorage.getItem('msg')) {
            this.arr = JSON.parse(localStorage.getItem('msg'));
        }
    },
    // computed通常會寫在mounted下面
    // 裡面的fn一定要寫return
    // 預處理拿到的資料，有暫存的功能，所以可以拿整包資料，利用判斷式將資料做篩選
    /*
    1.先在data宣告一個變數
    2.
     */
    computed: {
        selectedMsg() {
            return this.arr.filter((item) => {
                console.log(item);
                if (this.selectedTab === 'all') {
                    return this.arr;
                } else if (this.selectedTab === 'done') {
                    return item.checkBox == true;
                } else if (this.selectedTab === 'undone') {
                    return item.checkBox == false;
                }
            });
        }
    },
    methods: {
        //  切換頁面
        changeTab(tab) {
            this.selectedTab = tab;
        },
        // 新增資料
        addItem() {
            const id = this.arr.length || 0;
            this.obj = {
                id: id + 1,
                checkBox: false,
                msg: this.msg,
                // 編輯模式
                msgSwitch: false,
                // 編輯的文字
                editMsg: '',
            };
            this.arr.push(this.obj);
            // 清空輸入框
            this.msg = '';
            //localStorage只能存字串，故要先將陣列改成字串
            localStorage.setItem('msg', JSON.stringify(this.arr));
        },
        // 這個fn會無法清空輸入框
        // textInput(e) {
        //     // 取得e->event裡target的值
        //     this.msg = e.target.value;
        //     // console.log(this.obj.msg);
        // },

        // 刪除資料
        deleteItem(index) {
            // this.arr = this.arr.filter(deleteData => deleteData.id !== id);
            // console.log(this.arr);
            // 特定的索引值刪除幾個
            this.arr.splice(index, 1);
            localStorage.setItem('msg', JSON.stringify(this.arr));
        },
        // 編輯資料
        editItem(item) {
            console.log(item);
            // 編輯的按鈕開關
            item.msgSwitch = !item.msgSwitch;
            //如果編輯的文字是空的，就維持原本的文字
            if (item.editMsg !== '') {
                item.msg = item.editMsg;
            }
            if (item.msgSwitch === true) {
                item.editMsg = item.msg;
            }
            localStorage.setItem('msg', JSON.stringify(this.arr));
            // 用彈跳視窗的話，要用index去抓
            // this.arr[index].msg = prompt('請輸入');
        },
    }
};
</script>

<template>
    <div class="bg-blue-100 w-screen h-screen flex flex-col p-6">
        <div class="add w-[400px] h-[50px]">
            <input v-model="this.msg" type="text" placeholder="填寫新增事項" class=" h-[30px] w-[200px] rounded-md me-5">
            <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md" @click="addItem()">新增</button>
        </div>
        <div class="btn-all w-11/12 h-[45px] flex border-b-2 border-slate-500 gap-5">
            <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md"
                :class="{ 'active': selectedTab === 'all' }" @click="changeTab('all')">全部</button>
            <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md"
                :class="{ 'active': selectedTab === 'done' }" @click="changeTab('done')">已執行</button>
            <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md"
                :class="{ 'active': selectedTab === 'undone' }" @click="changeTab('undone')">未執行</button>
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

                <tr v-for="(item, index) in selectedMsg " :key="index" class="flex place-content-around">
                    <td><input type="checkbox"></td>
                    <td v-if="(item.msgSwitch === false)" class="font-bold">{{ item.msg }}</td>
                    <input v-else v-model="item.editMsg" type="text" class="border-2">
                    <td class="font-bold">
                        <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md"
                            @click="editItem(item)">編輯</button>
                        <button type="button" class="h-[30px] w-[80px] bg-slate-300 rounded-md"
                            @click="deleteItem(index)">刪除</button>
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