<template>
    <section class="task-detail-box">
        <div class="height-1"></div>
        <div class="weui-cells no-margin">
            <div class="weui-cell weui-cell_access">
                <div class="weui-cell__hd"><label class="weui-label">活动标题</label></div>
                <div class="weui-cell__bd">{{base.partyTitle}}</div>
            </div>
            <div class="weui-cell weui-cell_access show-message-box">
                <div class="weui-cell__hd"><label class="weui-label">开始时间</label></div>
                <div class="weui-cell__bd">{{base.planBeginTime}}</div>
            </div>
            <div class="weui-cell weui-cell_access show-message-box">
                <div class="weui-cell__hd"><label class="weui-label">结束时间</label></div>
                <div class="weui-cell__bd">{{base.planEndTime}}</div>
            </div>
            <div class="weui-cell weui-cell_access" @click="goToMap">
                <div class="weui-cell__hd"><label class="weui-label">活动地点</label></div>
                <div class="weui-cell__bd">{{base.addrDetail}}</div>
                <div class="weui-cell__ft"></div>
            </div>
        </div>

        <template v-if="couponData.length">
            <div class="wx-area-line"></div>
            <div class="weui-cells no-margin no-line">
                <div class="weui-cell weui-cell_access">
                    <div class="weui-cell__hd"><label class="weui-label">活动赠品</label></div>
                    <div class="weui-cell__bd wx-placeholder">
                       选择了{{couponData ? couponData.length : 0}}个赠品
                    </div>
                </div>
            </div>

            <div class="weui-cells no-margin left-padding">
                <a class="weui-media-box weui-media-box_appmsg"
                    v-for="(item, index) in couponData">
                <div class="weui-media-box__hd">
                    <img class="weui-media-box__thumb" :src="item.couponGroupCover">
                </div>
                <div class="weui-media-box__bd">
                    <h4 class="weui-media-box__title">{{item.couponGroupName}}</h4>
                    <p class="weui-media-box__desc">
                        {{item.couponGroutScenario}}
                    </p>
                </div>
            </a>
            </div>
        </template>

        <div class="wx-area-line"></div>
        <div class="weui-cells no-margin no-line">
            <div class="weui-cell weui-cell_access no-center">
                <div class="weui-cell__hd"><label class="weui-label">活动说明</label></div>
                <div class="weui-cell__bd">
                   {{base.partyDesc}}
                </div>
            </div>       
        </div>

        <div class="wx-area-line"></div>
        <div class="weui-cells no-margin no-line">
            <div class="weui-cell weui-cell_access">
                <div class="weui-cell__hd"><label class="weui-label">相关附件</label></div>
                <div class="weui-cell__bd wx-placeholder">
                   选择了{{attachmentData.pageData ? attachmentData.pageData.length : 0}}个附件
                </div>
            </div>
        </div>
        <attachment-show :attachment-data="attachmentData"></attachment-show>

        <template v-if="attachmentData.imgData.length">
            <div class="wx-area-line"></div>
            <div class="weui-cells no-margin no-line">
                <div class="weui-cell weui-cell_access">
                    <div class="weui-cell__hd"><label class="weui-label">附加图片</label></div>
                    <div class="weui-cell__bd wx-placeholder">
                       选择{{attachmentData.imgData.length}}张图片
                    </div>
                    <div class="weui-cell__ft"></div>
                </div>
            </div>
            <div class="weui-cells no-margin left-padding">
                <img-list :img-list="attachmentData.imgData"></img-list>
            </div>
        </template>
        <template v-if="base.partyStatus && base.partyStatus != '2' && base.partyStatus != '1'">
            <div class="wx-area-line"></div>
            <div class="weui-cells no-margin">
                <div class="weui-cell weui-cell_access">
                    <div class="weui-cell__hd"><label class="weui-label">新增会员</label></div>
                    <div class="weui-cell__bd">{{base.partyMemberCount}}人</div>
                </div>
            </div>
        </template>
        
        <!-- <template v-if="base.partyStatus == '4' || base.partyStatus == '5'">
            <div class="wx-area-line"></div>
            <div class="weui-cells no-margin no-line">
                <div class="weui-cell weui-cell_access no-center">
                    <div class="weui-cell__hd"><label class="weui-label">活动总结</label></div>
                    <div class="weui-cell__bd">
                       {{base.partySummary}}
                    </div>
                </div>       
            </div>

            <template v-if="partyImgs.length">
                <div class="wx-area-line"></div>
                <div class="weui-cells no-margin no-line">
                    <div class="weui-cell weui-cell_access">
                        <div class="weui-cell__hd"><label class="weui-label">附加图片</label></div>
                        <div class="weui-cell__bd wx-placeholder">
                           选择{{partyImgs.length}}张图片
                        </div>
                        <div class="weui-cell__ft"></div>
                    </div>
                </div>
                <div class="weui-cells no-margin left-padding">
                    <img-list :img-list="partyImgs"></img-list>
                </div>
            </template>
        </template> -->
        

        <div class="btn-height-box"></div>
        <template v-if="isPage && base.partyOwner == userInfo.userCode && base.partyStatus != '4' && base.partyStatus != '5'">
            <div class="weui-btn-area">
                <a class="weui-btn weui-btn_primary"
                    @click="showSheet">
                    管理推广活动
                </a>
            </div>
        </template>
        <template v-if="isPage && base.partyOwner == userInfo.userCode && (base.partyStatus == '4' || base.partyStatus == '5')">
            <div class="wx-bottom-nav">
                <a class="wx-nav-item-20" @click="goToChat">
                    沟通
                </a>
                <router-link class="wx-nav-item nav-blue"
                                :to="{
                                name: 'party-gift',
                                query: {
                                    enterpriseCode: $route.query.enterpriseCode,
                                    agentId: $route.query.agentId,
                                    partyCode: $route.query.partyCode,
                                    partyStatus: base.partyStatus
                                }
                            }">
                    进入活动现场
                </router-link>
            </div>
        </template>
        <template v-if="isPage && base.partyOwner != userInfo.userCode">
            <div class="wx-bottom-nav">
                <a class="wx-nav-item-20" @click="goToChat">
                    沟通
                </a>
                <router-link class="wx-nav-item nav-blue"
                                :to="{
                                name: 'party-gift',
                                query: {
                                    enterpriseCode: $route.query.enterpriseCode,
                                    agentId: $route.query.agentId,
                                    partyCode: $route.query.partyCode,
                                    partyStatus: base.partyStatus,
                                    partyAlbum: base.partyAlbum
                                }
                            }">
                    进入活动现场
                </router-link>
            </div>
        </template>

        <sheet :is-show-sheet="isShowSheet" :item-list="sheetList" :cb="publistOpt"></sheet>
    </section>
</template>
<script>
import imgList from '../common/imgList.vue'
import attachmentShow from '../common/attachmentShow.vue'
import util from '../../utils/tools'
import sheet from '../common/sheet.vue'
import { mapGetters, mapActions } from 'vuex'

export default {
    data () {
        return {
            isPage: false,
            base: {
                partyTitle: '',
                planBeginTime: '',
                planEndTime: '',
                addrLink: '',
                addrDetail: '',
                partyDesc: '',
                partySummary: '',
                partyAlbum: '',
                addrBaiduGps: '',
                partyMemberCount: ''
            },
            couponData: [],
            attachmentData: {
                sourceType: '',
                imgData: [],
                pageData: []
            },
            partyImgs: [],
            isShowSheet: {
                value: false
            },
            sheetList: []
        }
    },
    mounted () {
        this.getBase()
        this.getAttachments()
        this.setDetail({})
    },
    watch: {
        $route () {
            this.getBase()
            this.getAttachments()
            this.setDetail({})
        }
    },
    computed: {
        ...mapGetters({
            detailData: 'getDetail',
            userInfo: 'getUserInfo',
            userData: 'getUser'
        })
    },
    methods: {
        ...mapActions([
          'setDetail',
          'setUser'
        ]),
        showSheet () {
            this.isShowSheet.value = true
        },
        publistOpt (item) {
            if (item.pathName == 'close') {
                this.updateStatus('4')
            } else if (item.pathName == 'end') {
                this.updateStatus('5')
            } else if (item.pathName == 'start') {
                this.updateStatus('3')
            } else if (item.pathName == 'add') {
                this.gotoUser()
            } else if (item.pathName == 'edit') {
                var pathUrl = {
                        name: 'new-party',
                        query: {
                            enterpriseCode: this.$route.query.enterpriseCode,
                            agentId: this.$route.query.agentId,
                            partyCode: this.$route.query.partyCode
                        }
                    }

                this.$router.push(pathUrl)
            } else if (item.pathName == 'activity') {
                var pathUrl = {
                        name: 'party-gift',
                        query: {
                            enterpriseCode: this.$route.query.enterpriseCode,
                            agentId: this.$route.query.agentId,
                            partyCode: this.$route.query.partyCode,
                            partyStatus: this.base.partyStatus,
                            partyAlbum: this.base.partyAlbum
                        }
                    }

                this.$router.push(pathUrl)
            } else if (item.pathName == 'gotoWechat') {
                this.gotoWechat()
            }
        },
        getBase () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                partyCode: this.$route.query.partyCode
            }

            util.request({
                method: 'get',
                interface: 'partyInfoList',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                    this.base = res.result.result.partyInfo
                    if (res.result.result.couponGroup) {
                        this.couponData = res.result.result.couponGroup
                    }

                    if (this.base.partyStatus == '2' || this.base.partyStatus == '1') {
                        this.sheetList = [
                            {
                                label: '更新活动',
                                pathName: 'edit'
                            },
                            {
                                label: '取消活动',
                                pathName: 'end'
                            },
                            {
                                label: '启动活动',
                                pathName: 'start'
                            },
                            {
                                label: '邀请参与活动',
                                pathName: 'add'
                            },
                            {
                                label: '建群讨论',
                                pathName: 'gotoWechat'
                            }
                        ]
                    } else if (this.base.partyStatus == '3') {
                        this.sheetList = [
                            {
                                label: '结束活动',
                                pathName: 'close'
                            },
                            {
                                label: '邀请参与活动',
                                pathName: 'add'
                            },
                            {
                                label: '进入活动现场',
                                pathName: 'activity'
                            },
                            {
                                label: '建群讨论',
                                pathName: 'gotoWechat'
                            }
                        ]
                    } else {
                        this.getReviewList()
                    }

                    this.isPage = true
                    // 检查自动启动或结束
                    var nowDateStr = new Date().getTime()
                    var startDateStr = util.getDate(this.base.planBeginTime).getTime()
                    var stopDateStr = util.getDate(this.base.planEndTime).getTime()

                    if (['1', '2'].indexOf(this.base.partyStatus) > -1 && nowDateStr > startDateStr) {
                        this.updateStatus('3', true)
                    }

                    if (['4', '5'].indexOf(this.base.partyStatus) < 0 && nowDateStr > stopDateStr) {
                        this.updateStatus('4', true)
                    }
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        getAttachments () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                targetCode: this.$route.query.partyCode,
                targetType: 'party'
            }

            util.request({
                method: 'post',
                interface: 'getAttachments',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                    this.attachmentData = res.result.result
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        getReviewList () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                partyCode: this.$route.query.partyCode
            }

            util.request({
                method: 'get',
                interface: 'reviewList',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                    var partyImgs = []
                    res.result.result.materialLiblist.forEach((item) => {
                        partyImgs.push(item.fileCode)
                    })

                    this.partyImgs = partyImgs
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        updateStatus (status, type) {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                partyCode: this.$route.query.partyCode,
                agentId: this.$route.query.agentId,
                toUrl: window.location.href,
                partyStatus: status
            }

            util.request({
                method: 'post',
                interface: 'updateStatus',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                    if (status == '3' && !type) {
                        this.$message({
                            message: '活动已启动！',
                            type: 'success'
                        })
                    }

                    if (status == '4' && !type) {
                        this.$message({
                            message: '活动已结束！',
                            type: 'success'
                        })
                    }

                    if (status == '5' && !type) {
                        this.$message({
                            message: '活动已取消！',
                            type: 'success'
                        })
                    }
                    
                    this.getBase()
                    this.isShowSheet.value = false
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        goToMap () {
            var pathUrl = {
                name: 'search-map',
                query: {
                    enterpriseCode: this.$route.query.enterpriseCode,
                    agentId: this.$route.query.agentId,
                    point: this.base.addrBaiduGps
                }
            }
            this.$router.push(pathUrl)
        },
        gotoUser () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                partyCode: this.$route.query.partyCode
            }

            util.request({
                method: 'get',
                interface: 'partyAttendee',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                    var attData = {
                        userList: [],
                        userCodes: []
                    }

                    var userCodes = []
                    res.result.result.forEach((item) => {
                        userCodes.push(item.userCode)
                    })

                    attData.userCodes = [].concat(userCodes)

                    this.setUser(attData)

                    var urlPath = window.location.href

                    var pathUrl = {
                        name: 'user-list',
                        query: {
                            enterpriseCode: this.$route.query.enterpriseCode,
                            agentId: this.$route.query.agentId,
                            partyCode: this.$route.query.partyCode,
                            redirectUrl: window.encodeURIComponent(urlPath)
                        }
                    }
                    this.$router.push(pathUrl)
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        goToChat () {
            var attData = {
                userList: [],
                userCodes: [this.base.partyOwner]
            }

            this.setUser(attData)

            var urlPath = 'http://mobile.socialmarketingcloud.com/we-chat'
                urlPath = urlPath + '?enterpriseCode=' + this.$route.query.enterpriseCode + '&agentId=' + this.$route.query.agentId + '&partyCode=' + this.base.partyCode

            var pathUrl = {
                name: 'user-list',
                query: {
                    enterpriseCode: this.$route.query.enterpriseCode,
                    agentId: this.$route.query.agentId,
                    redirectUrl: window.encodeURIComponent(urlPath)
                }
            }
            this.$router.push(pathUrl)
        },
        gotoWechat () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                partyCode: this.$route.query.partyCode
            }

            util.request({
                method: 'get',
                interface: 'partyAttendee',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                    var attData = {
                        userList: [],
                        userCodes: []
                    }

                    var userCodes = []
                    res.result.result.forEach((item) => {
                        userCodes.push(item.userCode)
                    })
                    attData.userCodes = [].concat(userCodes)
                    attData.userCodes.push(this.base.partyOwner)
                    this.setUser(attData)

                    var urlPath = 'http://mobile.socialmarketingcloud.com/we-chat'
                    urlPath = urlPath + '?enterpriseCode=' + this.$route.query.enterpriseCode + '&agentId=' + this.$route.query.agentId + '&partyCode=' + this.base.partyCode

                    var pathUrl = {
                        name: 'user-list',
                        query: {
                            enterpriseCode: this.$route.query.enterpriseCode,
                            agentId: this.$route.query.agentId,
                            redirectUrl: window.encodeURIComponent(urlPath)
                        }
                    }
                    this.$router.push(pathUrl)
                } else {
                    this.$message.error(res.result.message)
                }
            })
        }
    },
    components: {
        imgList,
        attachmentShow,
        sheet
    }
}
</script>