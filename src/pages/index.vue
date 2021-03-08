<template>
    <div class="index">
        <h1>你好</h1>
        <p class="text">
            尊敬的
            <span style="color: red;">{{memberInfo}}用户</span>，欢迎来到德莱联盟！
        </p>

        <div class>
            <card
                :course="item"
                @goVideoList="goVideoList"
                v-for="(item, index) in courseList"
                :key="index"
            ></card>
        </div>
        <button class="footer-opt btn" @click="recharge">充值</button>
    </div>
</template>

<script>
import card from "../components/card";
import { mapGetters, mapState } from "vuex";
export default {
    components: {
        card
    },
    data() {
        return {
            courseList: []
        };
    },

    created() {
        // Mock todo
        this.courseList = [
            {
                id: "1",
                thumb:
                    "//img1.mukewang.com/5cb831fd0949d9f306000338-590-330.jpg",
                title: "学习vuex",
                description: "2312",
                charge: "",
                userStatus: 0,
                vipLevel: 0
            },
            {
                id: "2",
                thumb:
                    "//img1.mukewang.com/5cb831fd0949d9f306000338-590-330.jpg",
                title: "实战课程",
                description: "1231",
                charge: "实战课程",
                userStatus: 1,
                vipLevel: 0
            },
            {
                id: "3",
                thumb:
                    "//img1.mukewang.com/5cb831fd0949d9f306000338-590-330.jpg",
                title: "v12会员专享课程",
                description: "1123",
                charge: "v12会员专享",
                userStatus: 2,
                vipLevel: 12
            }
        ];
    },
    computed: {
        ...mapState(["userStatus", "vipLevel"]),
        ...mapGetters(["memberInfo"])
    },
    mounted() {},
    methods: {
        recharge() { //跳转
            this.$router.push("./userCenter");
        },
        goVideoList(e) {
            var _this = this;
            const res = this.checkPermission(e);
            // console.log(_this.$store.state.id);
            if (res) {
                this.$router.push({
                    name: "course",
                    params: {
                        id: e.id
                    }
                });
            } else if(!res && _this.$store.state.id == 2) { //第二权限
                alert("权限不足,无法观看,请分享免费视频获得权限");
            }else if(!res && _this.$store.state.id == 3) { //第三权限
                alert("权限不足,无法观看,请完成充值");
            }
        },
        checkPermission(e) {
            // console.log(e);
            this.$store.commit("setId", e.id);//点击进入id权限
            const userStatus = this.$store.state.userStatus;
            const vipLevel = this.$store.state.vipLevel;
            if (userStatus >= e.userStatus) {
                if (vipLevel >= e.vipLevel) {
                    return true;
                } else {
                    return false;
                }
            } else {
                return false;
            }
        }
    }
};
</script>

<style lang="less">
.index {
    padding: 0 20px 20px;
    box-sizing: border-box;
    h1,
    .text {
        text-align: left;
    }
    .footer {
        position: absolute;
        bottom: 0;
        left: 20px;
    }
    .btn {
        width: 100%;
        height: auto;
        color: #fff;
        background: #373737;
        margin: 10px 0 20px;
        padding: 15px;
        box-sizing: border-box;
        border-radius: 5px;
        font-size: 16px;
    }
}
</style>

