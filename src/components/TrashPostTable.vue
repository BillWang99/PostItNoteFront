<script>
    import axios from 'axios';

    export default{
        name:'TrashPostTable',
        data(){
            return {
                loading:true,
                postIt:{},
            }
        },
        methods:{
            //發API取得資料
            GetData(){
                //發送前先開啟loading動畫
                this.loading = true;
                axios.get('http://postitnote.com.tw/api/PostIt/AllDeletePost')
                .then(response =>{
                    const result = response;
                    //將資料寫入postIt物件
                    this.postIt = response.data;
                    //關閉loading動畫
                    this.loading = false;
                }).catch(error =>{
                    console.log('error', error);
                }).finally(() => {
                    console.log('完成');
                })
            },

            //還原移除的便利貼
            async rollbackPost(id){
                try{
                    const response = await axios.put('http://postitnote.com.tw/api/PostIt/RollBack/'+id,  {
                        headers: {
                            'Content-Type': 'application/json'
                        },
                    });
                    //重新整理畫面(再次取出資料)
                    this.GetData();
                }catch(error){
                    console.error('Error sending Put request:', error);
                }
            },

            //將移除的便利貼刪除
            async deletePost(id){
                try{
                    const response = await axios.delete('http://postitnote.com.tw/api/PostIt/Delete/'+id,  {
                        headers: {
                            'Content-Type': 'application/json'
                        },
                    });
                    //重新整理畫面(再次取出資料)
                    this.GetData();
                }catch(error){
                    console.error('Error sending delete request:', error);
                }
            }
        },
        mounted(){
            //模板掛載完成，透過API取得資料
            this.GetData();
        },
    }
</script>

<template>
    <!--loading-->
    <div class="loader" v-if="loading"></div>

    <!--便利貼列表-->
    <table class="table" v-if="!loading">
        <thead>
            <tr>
                <td>訊息</td>
                <td>操作</td>
            </tr>
        </thead>

        <tbody>
            <tr v-for="p in postIt">
                <td>{{ p.message }}</td>
                <td>
                    <span class="btn btn-outline-secondary me-2"><i class="bi bi-reply" @click="rollbackPost(p.id)"></i></span>
                    <span class="btn btn-outline-danger me-2"><i class="bi bi-trash" @click="deletePost(p.id)"></i></span>
                </td>
            </tr>
        </tbody>

    </table>
</template>

<style>
    .loader {
        position: absolute;
        top: 50%;
        left: 50%;
        margin: -60px 0 0 -60px;
        border: 16px solid #f3f3f3; /* Light grey */
        border-top: 16px solid #39775f; /* Blue */
        border-radius: 50%;
        width: 120px;
        height: 120px;
        animation: spin 2s linear infinite;
    }

    @keyframes spin {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    }
</style>