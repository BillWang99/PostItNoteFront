<script>
    import TrashPostTable from './TrashPostTable.vue';

    export default{
        name:'TrashCanView',
        data(){
            return {
                postIt:{},
            }
        },
        components:{
            //使用元件TrashPostTable動態取得列表狀態
            TrashPostTable,
        },
        methods:{
            //重新整理元件資料
            refreshData(){
                //使用子元件的function
                this.$refs.child.GetData();
            },
            //關閉視窗時重新整理畫面
            reloadPage(){
                location.reload();
            },
            
        }
    }
    
</script>

<template>
    <button class="trash-btn btn btn-outline-danger" data-bs-toggle="modal" data-bs-target="#trashModal" @click="refreshData()"><i class="bi bi-trash"></i></button>

    <!--modal-->
    <div class="modal fade" id="trashModal" tabindex="-1" aria-labelledby="trashModalModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="trashModalModalLabel">已丟棄的便利貼</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <TrashPostTable ref="child"/>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="reloadPage()">關閉</button>
            </div>
            </div>
        </div>
    </div>
</template>

<style>
    .trash-btn{
        position:absolute;
        top:80%;
        left:90%;
    }
</style>