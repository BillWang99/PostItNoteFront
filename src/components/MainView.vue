<script>
    import axios from 'axios';
    

    export default{
        name:'MainView',
        data(){
            return {
                postIt:{},
                loading:true,
                formData:{
                    message:'',
                }
            }
        },
        mounted(){
           this.GetData();
        },
        methods:{
            async GetData(){
                this.loading = true;
                await axios.get('http://postitnote.com.tw/api/PostIt/Get')
                .then(response =>{
                    const result = response;
                    this.postIt = response.data;
                    this.loading = false;
                    console.log(result);
                }).catch(error =>{
                    console.log('error', error);
                }).finally(() => {
                    console.log('完成');
                })
            },
            async AddPost(){
                try{
                    const response = await axios.post('http://postitnote.com.tw/api/PostIt/Post', this.formData, {
                        headers: {
                            'Content-Type': 'application/json'
                        },
                    });

                    //關閉modal
                    this.formData.message = '';
                    const myModalEl = document.getElementById('staticBackdrop');
                    const modal = bootstrap.Modal.getInstance(myModalEl); // Returns a Bootstrap modal instance                  
                    modal.hide();
                    

                    //關掉backdrop
                    const backdrop = document.querySelector('.modal-backdrop');
                    if (backdrop) {
                    backdrop.parentNode.removeChild(backdrop);
                    }

                    //重新整理畫面
                    //location.reload();
                    this.GetData();
                }catch(error){
                    console.error('Error sending POST request:', error);
                }
            },

            async RemovePost(id){
                try{
                    const response = await axios.put('http://postitnote.com.tw/api/PostIt/Put/'+id,  {
                        headers: {
                            'Content-Type': 'application/json'
                        },
                    });
                    //重新整理畫面
                    //location.reload();
                    this.GetData();
                }catch(error){
                    console.error('Error sending Put request:', error);
                }
            }
        }
        
    }
</script>

<template>
    <!--loader-->
    <div class="loader" v-if="loading"></div>

    <!--PostIt List-->
    <div id="mainArea" v-if="!loading">
        <div class="container">
            <button type="button" class="btn btn-secondary mt-2 mb-2" data-bs-toggle="modal" data-bs-target="#staticBackdrop">
                新增代辦事項
            </button>

            <div class="row">
                <div class="col-sm-3" v-for="p in postIt">
                    <div class="card text-bg-warning mb-3">
                    <div class="card-header"><i class="bi bi-x-lg removeBtn" @click="RemovePost(p.id)" v-bind="p.id"></i></div>
                    <div class="card-body">
                        <h5 class="card-text">{{ p.message }}</h5>
                    </div>
                    </div>
                </div>
            </div>
        </div>

        <!--Add Post modal-->
        <div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="staticBackdropLabel">新增便利貼</h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <form >
                            <input type="text" class="form-control" placeholder="輸入代辦事項..." v-model="formData.message">
                        </form>
                    </div>
                    <div class="modal-footer ">
                        <button type="button" class="btn btn-warning mx-auto" @click="AddPost">新增</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
</template>

<style>
    .container{
        max-width: 85%;
    }

    .row{
        max-width: 80%;
    }

    .removeBtn{
        cursor:pointer;
    }

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