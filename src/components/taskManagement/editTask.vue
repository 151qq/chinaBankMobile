<template>
    <section class="member-detail-box">
        <div class="height-1"></div>
        <div class="weui-cells no-margin show-message-box">
            <div class="weui-cell">
                <div class="weui-cell__hd"><label class="weui-label">任务标题</label></div>
                <div class="weui-cell__bd">
                    <input class="weui-input" placeholder="请输入文字" v-model="formData.taskTitle">
                </div>
                <div class="weui-cell__ft red-color">*</div>
            </div>

            <div class="weui-cell">
                <div class="weui-cell__hd"><label class="weui-label">开始时间</label></div>
                <div class="weui-cell__bd">
                    <input class="weui-input" placeholder="请选择" type="datetime-local" v-model="formData.taskBeginTimeD">
                </div>
                <div class="weui-cell__ft red-color">*</div>
            </div>

            <div class="weui-cell">
                <div class="weui-cell__hd"><label class="weui-label">结束时间</label></div>
                <div class="weui-cell__bd">
                    <input class="weui-input" placeholder="请选择" type="datetime-local" v-model="formData.taskEndTimeD">
                </div>
                <div class="weui-cell__ft red-color">*</div>
            </div>

            <div class="weui-cell">
                <div class="weui-cell__hd"><label class="weui-label">文章数量</label></div>
                <div class="weui-cell__bd">
                    <input class="weui-input"
                            v-model="formData.pageNum"
                            type="number"
                            pattern="[0-9]*"
                            placeholder="请输入">
                </div>
                <div class="weui-cell__ft red-color">*</div>
            </div>

            <div class="weui-cell weui-cell_select weui-cell_select-after">
                <div class="weui-cell__hd"><label class="weui-label">写作目的</label></div>
                <div class="weui-cell__bd">
                    <select class="weui-select" name="select1" v-model="formData.pageScenario">
                        <option value="" selected>无</option>
                        <option
                            v-for="(item, index) in scenarioList" :key="index"
                            :value="item.scenario">
                            {{item.scenarioKey}}
                        </option>
                    </select>
                </div>
            </div>

            <div class="weui-cell weui-cell_access" @click="showGenderSelect">
                <div class="weui-cell__hd"><label class="weui-label">客户性别</label></div>
                <div class="weui-cell__bd" v-if="formData.pageReaderGenderLabel">
                   {{formData.pageReaderGenderLabel}}
                </div>
                <div class="weui-cell__bd wx-placeholder" v-else>
                   请选择
                </div>
                <div class="weui-cell__ft"></div>
            </div>
            <checkbox-list :is-show-sheet="isGenderSheet"
                           :item-list="tagList.gender"
                           :map-data="valueMap"
                           ref="genderSelect"
                           @selectChange="genderChange"></checkbox-list>

            <div class="weui-cell weui-cell_access" @click="showAgeSelect">
                <div class="weui-cell__hd"><label class="weui-label">客户年龄</label></div>
                <div class="weui-cell__bd" v-if="formData.pageReaderAgeLabel">
                   {{formData.pageReaderAgeLabel}}
                </div>
                <div class="weui-cell__bd wx-placeholder" v-else>
                   请选择
                </div>
                <div class="weui-cell__ft"></div>
            </div>
            <checkbox-list :is-show-sheet="isAgeSheet"
                           :item-list="tagList.age"
                           :map-data="valueMap"
                           ref="ageSelect"
                           @selectChange="ageChange"></checkbox-list>

            <div class="weui-cell weui-cell_access" @click="showEducationSelect">
                <div class="weui-cell__hd"><label class="weui-label">教育背景</label></div>
                <div class="weui-cell__bd" v-if="formData.pageReaderEduLabel">
                   {{formData.pageReaderEduLabel}}
                </div>
                <div class="weui-cell__bd wx-placeholder" v-else>
                   请选择
                </div>
                <div class="weui-cell__ft"></div>
            </div>
            <checkbox-list :is-show-sheet="isEducationSheet"
                           :item-list="tagList.education"
                           :map-data="valueMap"
                           ref="educationSelect"
                           @selectChange="educationChange"></checkbox-list>

            <div class="weui-cell weui-cell_access" @click="showConsumeLevelSelect">
                <div class="weui-cell__hd"><label class="weui-label">消费能力</label></div>
                <div class="weui-cell__bd" v-if="formData.pageReaderConsumeLevel">
                   {{formData.pageReaderConsumeLevelLabel}}
                </div>
                <div class="weui-cell__bd wx-placeholder" v-else>
                   请选择
                </div>
                <div class="weui-cell__ft"></div>
            </div>
            <checkbox-list :is-show-sheet="isConsumeLevelSheet"
                           :item-list="tagList.consume_level"
                           :map-data="valueMap"
                           ref="consumeLevelSelect"
                           @selectChange="consumeLevelChange"></checkbox-list>

            <div class="weui-cell weui-cell_access" @click="showCareerSelect">
                <div class="weui-cell__hd"><label class="weui-label">客户职业</label></div>
                <div class="weui-cell__bd" v-if="formData.pageReaderCareerLabel">
                   {{formData.pageReaderCareerLabel}}
                </div>
                <div class="weui-cell__bd wx-placeholder" v-else>
                   请选择
                </div>
                <div class="weui-cell__ft"></div>
            </div>
            <checkbox-list :is-show-sheet="isCareerSheet"
                           :item-list="tagList.career_type"
                           :map-data="valueMap"
                           ref="careerSelect"
                           @selectChange="careerChange"></checkbox-list>

            <div class="weui-cell weui-cell_access" @click="showEnterpriseSelect">
                <div class="weui-cell__hd"><label class="weui-label">工作单位</label></div>
                <div class="weui-cell__bd" v-if="formData.pageReaderEnterpriseLabel">
                   {{formData.pageReaderEnterpriseLabel}}
                </div>
                <div class="weui-cell__bd wx-placeholder" v-else>
                   请选择
                </div>
                <div class="weui-cell__ft"></div>
            </div>
            <checkbox-list :is-show-sheet="isEnterpriseSheet"
                           :item-list="tagList.industry_type"
                           :map-data="valueMap"
                           ref="enterpriseSelect"
                           @selectChange="enterpriseChange"></checkbox-list>     
        </div>

        <div class="wx-area-line"></div>
        <div class="weui-cells no-margin no-line">
            <div class="weui-cell weui-cell_access no-center">
                <div class="weui-cell__hd"><label class="weui-label">任务详情</label></div>
                <div class="weui-cell__bd">
                   <textarea class="weui-textarea"
                        placeholder="请输入文字"
                        :rows="formData.taskDesc ? 3 : 1"
                        v-model="formData.taskDesc"></textarea>
                </div>
            </div>       
        </div>

        <div class="wx-area-line"></div>
        <div class="weui-cells no-margin no-line">
            <div class="weui-cell weui-cell_access" @click="gotoAttachment">
                <div class="weui-cell__hd"><label class="weui-label">相关附件</label></div>
                <div class="weui-cell__bd wx-placeholder">
                   已经选择了{{attachmentData.attachmentList ? attachmentData.attachmentList.length : 0}}个附件
                </div>
                <div class="weui-cell__ft"></div>
            </div>
        </div>
        <attachment-detail :attachment-data="attachmentData"></attachment-detail>

        <div class="wx-area-line"></div>
        <div class="weui-cells no-margin no-line show-message-box">
            <div class="weui-cell weui-cell_access">
                <div class="weui-cell__hd"><label class="weui-label">本地图片</label></div>
                <div class="weui-cell__bd wx-placeholder">
                   最多可以选择9张图片
                </div>
                <div class="weui-cell__ft"></div>
            </div>
        </div>

        <div class="weui-cells no-margin">
            <div class="weui-cell no-line">
                <div class="weui-uploader">
                    <div class="weui-uploader__bd">
                         <ul class="weui-uploader__files" id="uploaderFiles">
                            <li class="weui-uploader__file"
                                v-for="(item, index) in formData.imgData.localIds"
                                @click="showBigImg(index)">
                                    <img :src="item">
                            </li>
                            <li @click="chooseImage"
                                v-if="formData.imgData.localIds.length < 9"
                                class="weui-uploader__input-box"></li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>

        <div class="wx-area-line"></div>
        <div class="weui-cells no-margin no-line show-message-box">
            <div class="weui-cell weui-cell_access">
                <div class="weui-cell__hd"><label class="weui-label">任务封面</label></div>
                <div class="weui-cell__bd wx-placeholder">
                   请选择一张图片
                </div>
                <div class="weui-cell__ft"></div>
            </div>
        </div>

        <div class="weui-cells no-margin">
            <div class="weui-cell no-line">
                <div class="weui-uploader">
                    <div class="weui-uploader__bd">
                         <ul class="weui-uploader__files" id="uploaderFiles">
                            <li class="weui-uploader__file"
                                v-if="formData.localId"
                                @click="showBigImage">
                                    <img :src="formData.localId">
                            </li>
                            <li v-if="!formData.localId" @click="chooseImg" class="weui-uploader__input-box"></li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="btn-height-box"></div>
        <div class="weui-btn-area">
            <a class="weui-btn weui-btn_primary" @click="submitFn">分派任务</a>
        </div>

        <delete-img :index="nowIndex"
                    :img-path="nowPath"
                    :is-show-img="isShowImg"
                    @deleteImg="deleteImg"></delete-img>

        <delete-img :index="0"
                    :img-path="formData.localId"
                    :is-show-img="isShowImage"
                    @deleteImg="deleteImage"></delete-img>
    </section>
</template>
<script>
import util from '../../utils/tools'
import jsSdk from '../../utils/jsSdk'
import deleteImg from '../common/deleteImg.vue'
import attachmentDetail from '../common/attachmentDetail.vue'
import checkboxList from '../common/checkbox-list.vue'

import { mapGetters, mapActions } from 'vuex'

export default {
    data () {
        return {
            formData: {
                taskType: '1',
                taskTitle: '',
                taskBeginTime: '',
                taskEndTime: '',
                taskBeginTimeD: '',
                taskEndTimeD: '',
                taskDesc: '',
                pageNum: 1,
                pageScenario: '',
                pageReaderGender: '',
                // pageReaderCity: [],
                pageReaderAge: '',
                pageReaderEdu: '',
                pageReaderConsumeLevel: '',
                pageReaderCareer: '',
                pageReaderEnterprise: '',
                pageReaderGenderLabel: '',
                pageReaderAgeLabel: '',
                pageReaderEduLabel: '',
                pageReaderConsumeLevelLabel: '',
                pageReaderCareerLabel: '',
                pageReaderEnterpriseLabel: '',
                taskCover: '',
                localId: '',
                attachmentTargetType: 'editTask',
                imgData: {
                    localIds: [],
                    attachmentSourceType: 'attachmen_type_1',
                    attachmentSourceCodes: []
                },
                pageData: {
                    attachmentSourceType: '',
                    attachmentSourceCodes: []
                },
                codes: [],
                objectType: ''
            },
            valueMap: ['tagValue', 'tagValueCname'],
            nowIndex: '',
            nowPath: '',
            isShowImg: {
                value: false
            },
            isShowImage: {
                value: false
            },
            themesList: [],
            cityList: [],
            scenarioList: [],
            tagList: {
                age: [],
                consume_level: [],
                education: [],
                gender: [],
                industry_type: [],
                career_type: []
            },
            isGenderSheet: {
                value: false,
                selectData: {
                    values: [],
                    labels: []
                }
            },
            isAgeSheet: {
                value: false,
                selectData: {
                    values: [],
                    labels: []
                }
            },
            isEducationSheet: {
                value: false,
                selectData: {
                    values: [],
                    labels: []
                }
            },
            isConsumeLevelSheet: {
                value: false,
                selectData: {
                    values: [],
                    labels: []
                }
            },
            isCareerSheet: {
                value: false,
                selectData: {
                    values: [],
                    labels: []
                }
            },
            isEnterpriseSheet: {
                value: false,
                selectData: {
                    values: [],
                    labels: []
                }
            }
        }
    },
    mounted () {
        jsSdk.init()
        if (this.detailData.attachmentTargetType) {
            this.formData = Object.assign({}, this.detailData)

            setTimeout(() => {
                this.formData.taskBeginTimeD = this.detailData.taskBeginTimeD
                this.formData.taskEndTimeD = this.detailData.taskEndTimeD
            }, 0)
        } else {
            this.formData.taskBeginTimeD = util.formatDate(new Date().getTime(), 'yyyy-MM-ddThh:mm:ss')
        }
        this.getPageScenario()
        this.getTags()
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo',
            attachmentData: 'getAttachment',
            detailData: 'getDetail'
        })
    },
    methods: {
        ...mapActions([
          'setAttachment',
          'setDetail'
        ]),
        chooseImage () {
            jsSdk.chooseImage((localId ,serverId) => {
                this.formData.imgData.localIds.push(localId)
                this.formData.imgData.attachmentSourceCodes.push(serverId)
            })
        },
        chooseImg () {
            jsSdk.chooseImage((localId, serverId) => {
                this.formData.localId = localId
                this.formData.taskCover = serverId
            })
        },
        submitFn () {
            if (!this.formData.taskTitle) {
                this.$message({
                    message: '请填写任务标题!',
                    type: 'warning'
                })
                return false
            }

            // this.formData.taskBeginTime = '2017-01-02 11:11:11'
            // this.formData.taskEndTime = '2017-01-05 11:11:11'

            if (!this.formData.taskBeginTimeD) {
                this.$message({
                    message: '请填写开始时间!',
                    type: 'warning'
                })
                return false
            }

            if (!this.formData.taskEndTimeD) {
                this.$message({
                    message: '请填写结束时间!',
                    type: 'warning'
                })
                return false
            }

            if (util.getDate(this.formData.taskBeginTimeD).getTime() > util.getDate(this.formData.taskEndTimeD).getTime()) {
                this.$message({
                    message: '开始应小于结束时间!',
                    type: 'warning'
                })
                return false
            }

            if (!this.formData.pageNum) {
                this.$message({
                    message: '请填写文章数量!',
                    type: 'warning'
                })
                return false
            }

            this.formData.taskBeginTime = util.formatDate(this.formData.taskBeginTimeD, 'yyyy-MM-dd hh:mm:ss')
            this.formData.taskEndTime = util.formatDate(this.formData.taskEndTimeD, 'yyyy-MM-dd hh:mm:ss')

            var formData = Object.assign({}, this.formData)
            formData.userCode = this.userInfo.userCode
            formData.enterpriseCode = this.$route.query.enterpriseCode
            formData.agentId = this.$route.query.agentId
            formData.pageData.attachmentSourceType = this.attachmentData.targetType
            formData.pageData.attachmentSourceCodes = this.attachmentData.attachmentCodes

            if (['XPTJ', 'CPCX'].indexOf(formData.pageScenario) > -1) {
                formData.objectType = 1
            }

            if (['DTHDXC', 'XSXF', 'XXYL'].indexOf(formData.pageScenario) > -1) {
                formData.objectType = 2
            }

            util.request({
                method: 'post',
                interface: 'saveTask',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                    this.setDetail({})
                    this.setAttachment({})
                    this.gotoUser(res.result.result)
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        getTags () {
            util.request({
                method: 'get',
                interface: 'selectAllTagDef',
                data: {}
            }).then(res => {
                if (res.result.success == '1') {
                    this.tagList = res.result.result
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        getPageScenario () {
            util.request({
                method: 'get',
                interface: 'getPageScenario',
                data: {}
            }).then(res => {
                if (res.result.success == '1') {
                    this.scenarioList = res.result.result
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        gotoAttachment () {
            this.setDetail(Object.assign({}, this.formData))

            var pathUrl = {
                name: 'attachment-list',
                query: {
                    enterpriseCode: this.$route.query.enterpriseCode,
                    agentId: this.$route.query.agentId
                }
            }
            this.$router.push(pathUrl)
        },
        gotoArticle () {
            this.setDetail(Object.assign({}, this.formData))

            var pathUrl = {
                name: 'product-attachment',
                query: {
                    enterpriseCode: this.$route.query.enterpriseCode,
                    agentId: this.$route.query.agentId,
                    isPage: 1,
                    redirectUrl: window.encodeURIComponent(window.location.href)
                }
            }
            this.$router.push(pathUrl)
        },
        gotoParty () {
            this.setDetail(Object.assign({}, this.formData))

            var pathUrl = {
                name: 'party-attachment',
                query: {
                    enterpriseCode: this.$route.query.enterpriseCode,
                    agentId: this.$route.query.agentId,
                    isPage: 1,
                    redirectUrl: window.encodeURIComponent(window.location.href)
                }
            }
            this.$router.push(pathUrl)
        },
        gotoUser (taskCode) {
            var urlPath = window.location.href.replace('editTask', 'editDetail')
            urlPath = urlPath + '&taskCode=' + taskCode

            var pathUrl = {
                name: 'user-list',
                query: {
                    enterpriseCode: this.$route.query.enterpriseCode,
                    agentId: this.$route.query.agentId,
                    taskCode: taskCode,
                    taskType: '1',
                    taskTitle: this.formData.taskTitle,
                    redirectUrl: window.encodeURIComponent(urlPath)
                }
            }
            this.$router.replace(pathUrl)
        },
        showBigImg (index) {
            this.nowIndex = index
            this.nowPath = this.formData.imgData.localIds[index]
            this.isShowImg.value = true
        },
        deleteImg (index) {
            this.formData.imgData.localIds.splice(index, 1)
            this.formData.imgData.attachmentSourceCodes.splice(index, 1)
        },
        showBigImage () {
            this.isShowImage.value = true
        },
        deleteImage () {
            this.formData.taskCover = ''
            this.formData.localId = ''
        },
        showGenderSelect () {
            this.$refs.genderSelect.resetData()
            this.isGenderSheet.value = true
        },
        genderChange (data) {
            this.isGenderSheet.selectData = data
            this.formData.pageReaderGender = data.values.join(',')
            this.formData.pageReaderGenderLabel = data.labels.join(',')
        },
        showAgeSelect () {
            this.$refs.ageSelect.resetData()
            this.isAgeSheet.value = true
        },
        ageChange (data) {
            this.isAgeSheet.selectData = data
            this.formData.pageReaderAge = data.values.join(',')
            this.formData.pageReaderAgeLabel = data.labels.join(',')
        },
        showEducationSelect () {
            this.$refs.educationSelect.resetData()
            this.isEducationSheet.value = true
        },
        educationChange (data) {
            this.isEducationSheet.selectData = data
            this.formData.pageReaderEdu = data.values.join(',')
            this.formData.pageReaderEduLabel = data.labels.join(',')
        },
        showConsumeLevelSelect () {
            this.$refs.consumeLevelSelect.resetData()
            this.isConsumeLevelSheet.value = true
        },
        consumeLevelChange (data) {
            this.isConsumeLevelSheet.selectData = data
            this.formData.pageReaderConsumeLevel = data.values.join(',')
            this.formData.pageReaderConsumeLevelLabel = data.labels.join(',')
        },
        showCareerSelect () {
            this.$refs.careerSelect.resetData()
            this.isCareerSheet.value = true
        },
        careerChange (data) {
            this.isCareerSheet.selectData = data
            this.formData.pageReaderCareer = data.values.join(',')
            this.formData.pageReaderCareerLabel = data.labels.join(',')
        },
        showEnterpriseSelect () {
            this.$refs.enterpriseSelect.resetData()
            this.isEnterpriseSheet.value = true
        },
        enterpriseChange (data) {
            this.isEnterpriseSheet.selectData = data
            this.formData.pageReaderEnterprise = data.values.join(',')
            this.formData.pageReaderEnterpriseLabel = data.labels.join(',')
        }
    },
    components: {
        deleteImg,
        attachmentDetail,
        checkboxList
    }
}
</script>