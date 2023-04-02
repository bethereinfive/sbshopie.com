<template>
    <div>

        <div class="layout-content" style="margin-bottom: 100px;">



            <section id="listmenus" style="margin-top: 60px;">
            <div role="feed" class="van-list row" aria-busy="true">
                <div class="col-md-12" v-for="task in row.task" :key="'task'+task.id">
                    <div class="card" :style="{backgroundColor:'#0077C3'}" style="margin:10px;margin-bottom: 20px">
                        <div class="card-body">
                            <p class="text-white">Date： {{ dateformatglobal(task.created_at)[6] }}</p>
                            <p class="text-white">Task Comisition： {{ task.task_comisition }} TK</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>


            </div>


    </div>
</template>
<script>
export default {
    data() {
        return {
            row: {},
        }
    },
    methods: {
        generator() {
            return '#' + (Math.random() * 0xFFFFFF << 0).toString(16);
        },
        async getData() {
            var id = localStorage.getItem('userid');
            var res = await this.callApi('get', `/api/admin/task?id=${id}`, []);
            this.row = res.data;
        },
    },
    mounted() {
        this.getData();
    },
}
</script>
<style>
.recordDiv.p-3 {
    border-bottom: 1px solid white;
}
</style>
