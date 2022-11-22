<template>
    <div>
    <div>
    英文论文
    </div>
    <div>
        <el-button @click="showViewVisible">预览</el-button>
    </div>
    <el-dialog title="预览" :visible.sync="ViewVisible" width="100%" height="100%" :before-close='closeDialog'>
    <div class="pdf" v-show="fileType === 'pdf'">
        <pdf :src="testpdf" ></pdf>
    </div>
   </el-dialog>
    </div>
</template>

<script>
    import pdf from 'vue-pdf'

    export default {
    name: "english",
    components: {
        pdf
    },
    data(){
    return{
        ViewVisible:false,
        fileType: 'pdf', // 文件类型
        testpdf:'/static/diagram.pdf'
    }
    },
    methods: {
    showViewVisible(){
        this.ViewVisible = true
    },
    closeDialog(done) {
    done();
    },
    }
    }
    </script>

      <style scoped>

</style>
