<template>
<!--    <a-list :grid="{ gutter: 16, xs: 1, sm: 2, md: 4, lg: 4, xl: 6, xxl: 3 }" :data-source="internship_data">-->
    <a-list :grid="{ gutter: 16, column: 1}" :data-source="internship_data">
        <a-list-item slot="renderItem" slot-scope="item, index">
            <a-card :title="`我的实习项目${index+1}`">
                <a-descriptions :title=item.name bordered>
                    <a-descriptions-item label="项目编号">
                        {{item.project_id}}
                    </a-descriptions-item>
                    <a-descriptions-item label="负责人">
                        {{item.leader}}
                    </a-descriptions-item>
                    <a-descriptions-item label="地址" span="2">
                        {{item.address}}
                    </a-descriptions-item>
                    <a-descriptions-item label="已招人数/招收人数">
                        {{item.recoreded}}/{{item.number}}
                    </a-descriptions-item>
                    <a-descriptions-item label="项目状态">
                        {{item.status}}
                    </a-descriptions-item>
                    <a-descriptions-item v-if="item.tags!=null" label="标签">
                        <a-tag v-for="tag in item.tags.split('|')" :key=tag color="grey">
                            {{tag}}
                        </a-tag>
                    </a-descriptions-item>
                    <a-descriptions-item label="详细信息">
                        {{item.info}}
                    </a-descriptions-item>
                    <a-descriptions-item label="研究生院">
                        {{internship_audit_number2title[item.school_audit]}}
                    </a-descriptions-item>
                    <a-descriptions-item label="基地">
                        {{internship_audit_number2title[item.base_audit]}}
                    </a-descriptions-item>
                    <a-descriptions-item label="导师1">
                        {{internship_audit_number2title[item.tutor1_audit]}}
                    </a-descriptions-item>
                    <a-descriptions-item label="导师2">
                        {{internship_audit_number2title[item.tutor2_audit]}}
                    </a-descriptions-item>
                </a-descriptions>
            </a-card>
        </a-list-item>
    </a-list>
</template>

<script>
    import {mapGetters} from "vuex";
    import {IMSget} from "../../api/user";

    export default {
        name: "yan-myinternship",

        mounted: function() {
            this.init_data();
        },

        computed: {
            ...mapGetters([
                'internship_data',
                'internship_audit_number2title',
            ])
        },

        methods: {
            init_data() {
                IMSget("/student/getInternships").then(res => {
                    if(res.success == true) {
                        this.$store.dispatch('pro_update_internship_data', res.data);
                    }
                })
            }
        }
    }
</script>

<style scoped>

</style>