<template>
    <section>
        <common-table
                :queryapi="queryapi"
                :addapi="addapi"
                :editapi="editapi"
                :delapi="delapi"
                :tableheight="tableheight"
                :fieldsstr="fieldsstr"
                :tableitems="tableitems"
                :btswidth="btswidth"
                :hide-export="hideExport"
                :hide-options="hideOptions"
                :addtitle="addtitle"
                :showEdit="showEdit"
                :showdateSelector="showdateSelector"
                :hideTool="hideTool"
                :showParkInfo="showParkInfo"
                :hideSearch="hideSearch"
                :hideAdd="hideAdd"
                :showdelete="showdelete"
                ref="bolinkuniontable"
        ></common-table>

    </section>
</template>

<script>

    import {path, checkURL, checkUpload, checkNumber,
        moveStyle, moveSpeed, waitingTime, LEDColor,
        color, fontStyle, fontSize, LEDStyle, redirectPort} from '../../api/api';
    import util from '../../common/js/util'
    import common from '../../common/js/common'
    import CommonTable from '../../components/CommonTable'

    export default {
        components: {
            CommonTable      //表格
        },
        data() {
            return {
                loading: false,         //loading页面是否显示
                hideExport: true,       //隐藏导出
                hideSearch: true,      //隐藏查询
                showdateSelector: true,//显示日期查询
                hideAdd: false,         //隐藏添加
                tableheight: '',        //表格高度
                showdelete: true,       //显示删除
                hideOptions: false,      //隐藏多选框
                showParkInfo: false,     //显示停车信息
                hideTool: false,        //隐藏工具栏
                showEdit:true,
                queryapi: '/EQ_LED/query',    //数据请求路径
                addapi: '/EQ_LED/add',
                editapi: '/EQ_LED/edit',
                delapi: '/EQ_LED/remove',
                btswidth: '100',                 //按钮宽度
                fieldsstr: 'ledip__ledport__leduid__movemode__movespeed__dwelltime__ledcolor__showcolor__typeface__typesize__matercont' +
                '__width__height__type__rsport__worksite_id__passid__limit_time__resume',//请求数据的格式，在云平台的页面找接口和有关请求参数。
                tableitems: [                       //表格元素，表头
                    {

                        hasSubs: false,
                        subs: [{
                            label: 'IP地址',          //页面表格显示
                            prop: 'ledip',             //对应表中字段
                            width: '130',           //列宽度
                            type: 'str',         //对应表中字段类型
                            editable: true,         //是否可编辑
                            searchable: true,       //是否可查询
                            addable: true,          //是否可添加
                            unsortable: true,       //是否可排序
                            align: 'center'         //页面表格内容显示位置
                        }]
                    }, {

                        hasSubs: false,
                        subs: [{
                            label: '端口号',
                            prop: 'ledport',
                            width: '100',
                            type: 'str',
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    }, {

                        hasSubs: false,
                        subs: [{
                            label: '素材UID',
                            prop: 'leduid',
                            width: '100',
                            type: 'str',
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    }, {

                        hasSubs: false,
                        subs: [{
                            label: '移动方式',
                            prop: 'movemode',
                            width: '100',
                            type: 'selection',
                            selectlist:moveStyle,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center',
                        }]
                    }, {

                        hasSubs: false,
                        subs: [{
                            label: '移动速度',
                            prop: 'movespeed',
                            width: '120',
                            type: 'selection',
                            selectlist:moveSpeed,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center',
                        }]
                    }, {

                        hasSubs: false,
                        subs: [{
                            label: '停留时间',
                            prop: 'dwelltime',
                            width: '100',
                            type: 'selection',
                            selectlist:waitingTime,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '显示屏颜色',
                            prop: 'ledcolor',
                            width: '110',
                            type: 'selection',
                            selectlist:LEDColor,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '颜色',
                            prop: 'showcolor',
                            width: '70',
                            type: 'selection',
                            selectlist:color,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '字体',
                            prop: 'typeface',
                            width: '70',
                            type: 'selection',
                            selectlist:fontStyle,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '字号',
                            prop: 'typesize',
                            width: '70',
                            type: 'selection',
                            selectlist:fontSize,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '素材内容',
                            prop: 'matercont',
                            width: '300',
                            type: 'str',
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '宽',
                            prop: 'width',
                            width: '100',
                            type: 'number',
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '高',
                            prop: 'height',
                            width: '100',
                            type: 'number',
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '类型',
                            prop: 'type',
                            width: '70',
                            type: 'selection',
                            selectlist:LEDStyle,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '转发端口',
                            prop: 'rsport',
                            width: '100',
                            type: 'selection',
                            selectlist:redirectPort,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '所属工作站',
                            prop: 'worksite_id',
                            width: '120',
                            type: 'selection',
                            //selectlist:channlManager,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{
                        hasSubs: false,
                        subs: [{
                            label: '所属通道',
                            prop: 'passid',
                            width: '100',
                            type: 'selection',
                            //selectlist:channlManager,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },
                ],

                addtitle: '添加LED屏',
                searchtitle: '查询明细',


            }
        },
        mounted() {
            window.onresize = () => {
                this.tableheight = common.gwh() - 143;
            };
            this.tableheight = common.gwh() - 143;

        },
        activated() {
            window.onresize = () => {
                this.tableheight = common.gwh() - 143;
            };
            this.tableheight = common.gwh() - 143;
            this.$refs['bolinkuniontable'].$refs['search'].resetSearch();
            this.$refs['bolinkuniontable'].getTableData({})
        },
        methods: {}
    }
</script>

<style scoped>
    .gutter {
        display: none
    }
</style>