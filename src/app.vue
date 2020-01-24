<template>
    <div style="overflow:hidden;">
        <div style="width:100%;font-size:40px;line-height:48px;color:green;">
            滚动条展示
        </div>
        <div :style="styleOpt" id="listwrapper" class="listwrapper">
            <div class="list" id="list">
                <div v-for="item in list" class="item">
                    {{item}}
                </div>
            </div>
        </div>
        <div style="width:100%;font-size:40px;line-height:48px;border-bottom: solid 1px;color:green;">
            底部条展示
        </div>
    </div>
</template>
<script lang="ts">
    import { Vue, Component } from "vue-property-decorator"
    const PhyTouch = require("phy-touch")
    const Transform = require("css3transform")

    @Component({ name: "App" })
    export default class App extends Vue{
        list: string[] = []
        styleOpt: any = { "height": "0" }
        bindTouch() {
            var target = <HTMLElement>document.getElementById("list")
            Transform(target, true)
            var phy = new PhyTouch({
                touch:"#listwrapper",//反馈触摸的dom
                vertical: true,//不必需，默认是true代表监听竖直方向touch
                target: target, //运动的对象
                property: "translateY",  //被滚动的属性
                sensitivity: 1,//不必需,触摸区域的灵敏度，默认值为1，可以为负数
                factor: 1,//不必需,默认值是1代表touch区域的1px的对应target.y的1
                min: window.innerHeight - 45 - 48 - target.scrollHeight, //不必需,滚动属性的最小值
                max: 0, //不必需,滚动属性的最大值
                step: 40
            })
        }

        mounted() {
            for (var i = 0; i < 25; ++i) {
                this.list.push("row" + i.toString())
            }

            this.styleOpt["height"] = (window.innerHeight - 96).toString() + "px"
            document.body.addEventListener("touchmove", function (evt) {
                evt.preventDefault();
            }, false);
            Vue.nextTick(() => {
                this.bindTouch()
            })
        }
    }
</script>
<style scoped lang="scss">
    .listwrapper {
        background:#ccc;
        overflow:hidden;
    }

    .list {
        width:100%;
    }

    .item {
        line-height: 40px;
        font-size:32px;
        border-bottom-width:1px;
        border-bottom-color:black;
        border-bottom-style:solid;
    }
</style>