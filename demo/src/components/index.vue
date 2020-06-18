<template>
  <div class="hello">
    <div class="inputInterface">
      <input
        style="outline: none;"
        type="text"
        v-model="textCen"
        @keyup.enter="onSubmit"
        placeholder="请输入内容"
      />
    </div>
    <!-- 正在进行 -->
    <div class="underway">
      <h3>
        正在进行 <span>操作</span> <span>{{ underwayNumber }}</span>
      </h3>

      <ul v-for="(item, index) in underwayList" :key="index">
        <li class="underwayLi">
          <input
            type="text"
            v-model="inputCen1"
            style="outline: none;"
            v-if="isShow || CIndex1 === index"
          />
          <p v-else>{{ item }}</p>
          <button class="add" @click="underwayAdd(index, item)">添加</button>
          <button
            v-if="isShow || CIndex1 === index"
            class="change"
            @click="underwayCompile1(index, item)"
          >
            确认
          </button>
          <button v-else class="change" @click="underwayCompile(index, item)">
            编辑
          </button>
          <button class="del" @click="underwayDele(index)">删除</button>
        </li>
      </ul>
    </div>

    <!-- 已经完成 -->
    <div class="haveFinished">
      <h3>
        已经完成 <span>操作</span> <span>{{ haveFinishedNumber }}</span>
      </h3>
      <ul v-for="(item, index) in haveFinishedList" :key="index">
        <li class="underwayLi">
          <input
            type="text"
            style="outline: none;"
            v-model="inputCen2"
            v-if="isShow || CIndex2 === index"
          />
          <p v-else>{{ item }}</p>
          <button class="add" @click="finishedAdd(index, item)">添加</button>
          <button
            v-if="isShow || CIndex2 === index"
            class="change"
            @click="finishedCompile1(index, item)"
          >
            确认
          </button>
          <button v-else class="change" @click="finishedCompile(index, item)">
            编辑
          </button>
          <button class="del" @click="finishedDele(index)">删除</button>
        </li>
      </ul>
    </div>
    <Timer color="pink" @end="endfn"></Timer>
  </div>
</template>

<script>
import Timer from "@/components/time.vue";
export default {
  data() {
    return {
      isShow: false, // 控制显示与隐藏
      CIndex1: "", // 当前索引
      CIndex2: "",
      textCen: "", // 输入框的值
      inputCen1: "",
      inputCen2: "",
      underwayList: [], // 正在进行的数组
      haveFinishedList: [], // 已经完成的数组
      underwayNumber: 0, // 进行时的数量
      haveFinishedNumber: 0 // 已经完成的数量
    };
  },
  components: {
    Timer
  },
  methods: {
    //倒计时
    endfn() {
      console.log("倒计时结束了！！！");
    },
    // 回车事件
    onSubmit() {
      if (this.textCen === "" || this.textCen === null) {
        return;
      }
      this.underwayList.unshift(this.textCen);
      this.textCen = null;
      this.underwayNumber++;
    },

    // 正在进行的添加事件
    underwayAdd(currentIndex, currentItem) {
      this.underwayNumber--;
      this.underwayList.splice(currentIndex, 1);
      this.haveFinishedList.unshift(currentItem);
      this.haveFinishedNumber++;
    },

    // 正在进行的修改事件
    underwayCompile(currentIndex, currentItem) {
      this.CIndex1 = currentIndex;
      this.inputCen1 = currentItem;
    },
    // 确认修改事件
    underwayCompile1(currentIndex) {
      this.CIndex1 = false;
      this.underwayList[currentIndex] = this.inputCen1;
    },
    // 正在进行的删除事件
    underwayDele(currentIndex) {
      this.underwayList.splice(currentIndex, 1);
      this.underwayNumber--;
    },

    // 已经完成的添加事件
    finishedAdd(currentIndex, currentItem) {
      this.underwayNumber++;
      this.haveFinishedList.splice(currentIndex, 1);
      this.underwayList.unshift(currentItem);
      this.haveFinishedNumber--;
    },

    // 已经完成的修改事件
    finishedCompile(currentIndex, currentItem) {
      this.CIndex2 = currentIndex;
      this.inputCen2 = currentItem;
    },
    finishedCompile1(currentIndex) {
      this.CIndex2 = false;
      this.haveFinishedList[currentIndex] = this.inputCen2;
    },
    // 已经完成的删除事件
    finishedDele(currentIndex) {
      this.haveFinishedList.splice(currentIndex, 1);
      this.haveFinishedNumber--;
    }
  }
};
</script>

<style scoped>
@import "../assets/reset.css";
.hello {
  width: 600px;
  height: 600px;
  margin: 0 auto;
}

.inputInterface input {
  outline: none;
  width: 350px;
  height: 35px;
}

/* 正在进行时 */
.underway,
.haveFinished {
  margin: 25px 0;
  width: 100%;
  text-align: left;
}

.underway > h3,
.haveFinished > h3 {
  display: flex;
  justify-content: space-between;
}

.underway > h3 > span:first-child,
.haveFinished > h3 > span:first-child {
  margin-left: 320px;
}

.underway > h3 > span:last-child,
.haveFinished > h3 > span:last-child {
  display: inline-block;
  padding: 0 5px;
  height: 20px;
  border-radius: 20px;
  background: #e6e6fa;
  line-height: 22px;
  text-align: center;
  color: #666;
  font-size: 14px;
}

.underwayLi {
  display: flex;
  align-items: center;
  height: 30px;
  margin: 10px 0;
}

.underwayLi > p,
.underwayLi > input {
  width: 400px;
}

.underwayLi button {
  border: none;
  background-color: #ffffff;
  margin: 10px;
  width: 85px;
  height: 30px;
  line-height: 30px;
  text-align: center;
  border-radius: 5px;
  cursor: pointer;
  outline: 0;
  color: #fff;
}

.underwayLi .add {
  background-color: #66b1ff;
}

.underwayLi .change {
  background-color: #a6a9ad;
}

.underwayLi .del {
  background-color: #f78989;
}
</style>
