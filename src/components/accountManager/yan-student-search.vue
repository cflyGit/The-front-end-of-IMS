<template>
    <div class="student-search">
        <a-select :default-value=st_search[0] style="width: 100px" @change="handleChange">
            <a-select-option v-for="(item, index) in st_search" :key=index>
                {{item}}
            </a-select-option>
        </a-select>

        <a-input-search placeholder="搜索" style="width: 400px" @search="onSearch" :loading="loading" enter-button />

        <a-button-group class="button-group">
            <a-button type="primary" @click="toEnroll">信息录入<a-icon type="right" /> </a-button>
        </a-button-group>
    </div>
</template>

<script>
    import {mapGetters} from "vuex";
    import {search} from '@/api/user'
    import {json2array} from "@/store/modules/json2array";

    export default {
        name: "yan-student-search",

        data() {
            return {
                loading: false,
                tag: "Id",
            };
        },

        computed: {
            ...mapGetters([
                'st_search',
                'st_search_en'
            ])
        },

        methods: {
            //搜索框点击事件
            onSearch(value) {
                var that = this;
                //设置加载中状态
                that.loading = true;

                search("student/findBy" + this.tag + "/" + value).then(res=>{
                    that.loading = false;
                    if (res.success !== undefined && res.success === true) {
                        this.$message.success("查询成功");
                        this.$store.commit('st_update_table_data', json2array(res.data))
                    }else {
                        this.$store.commit('st_update_table_data', null);
                    }
                });
                setTimeout(function () {
                    that.loading = false;
                }, 3000);
            },

            handleChange(value){
                this.tag = this.st_search_en[value];
            },

            toEnroll() {
                this.$router.push("/user/student/enroll");
            }
        },

    }
</script>

<style scoped>
    .student-search{
        text-align: center;
        padding: 20px;
    }

    .button-group{
        float: right;
    }
</style>