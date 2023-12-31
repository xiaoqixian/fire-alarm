<template>
  <div class="page-switching">
    <div class="flip-page">
      <div 
        class="page-button prev-page" 
        :class="{'disabled-button': prevPageDisabled}"
        @click="prevPage"
        ref="prevPageButton"
      ></div>
      <div class="page-index">{{pageIndex}}/{{totalPage}}</div>
      <div 
        class="page-button next-page" 
        :class="{'disabled-button': nextPageDisabled}"
        @click="nextPage"
        ref="nextPageButton"
      ></div>
    </div>
    <div class="jump-page">
      <div class="label">跳转</div>
      <div class="page-search">
        <input 
          type="number"
          min="1"
          :max="totalPage"
          v-model="jumpPageIndex"
          ref="jumpPageComp"
          @keyup="jumpPage"
        />
      </div>
      <div class="label">页</div>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue';

export default {
  name: 'PageSwtiching',
  props: {
    initialPage: {type: Number,default: 1},
    totalPage: {type: Number, required: true},
  },
  setup: function(props) {
    const minPage = Math.min(1, props.totalPage);//totalPage can be 0
    return {
      minPage,
      pageIndex: ref(props.initialPage),
      prevPageDisabled: ref(props.initialPage == minPage),
      nextPageDisabled: ref(props.initialPage == props.totalPage),
      jumpPageIndex: ref(null),
    };
  },
  watch: {
    pageIndex: function(newVal) {
      this.prevPageDisabled = newVal == this.minPage ? true : false;
      this.nextPageDisabled = newVal == this.$props.totalPage ? true : false;
    },
    totalPage: function(newVal) {
      //when totalPage is altered, it means new pages are inserted
      //so rewind pageindex to 1
      this.minPage = Math.min(1, newVal);

      if (this.pageIndex == this.minPage) {
        this.prevPageDisabled = true;
        this.nextPageDisabled = newVal == this.minPage ? true : false;
      } else {
        this.pageIndex = this.minPage;
      }
      this.$refs.jumpPageComp.max = newVal;
    }
  },
  emits: ["jumpPage"],
  methods: {
    prevPage: function() {
      this.pageIndex--;
      this.$emit("jumpPage", this.pageIndex);
    },
    nextPage: function() {
      this.pageIndex++;
      this.$emit("jumpPage", this.pageIndex);
    },
    jumpPage: function(event) {
      if (
        event.key == "Enter" && 
        this.pageIndex != this.jumpPageIndex &&
        this.jumpPageIndex >= 1 &&
        this.jumpPageIndex <= this.totalPage
      ) {
        this.pageIndex = this.jumpPageIndex;
        this.jumpPageIndex = null;
        this.$emit("jumpPage", this.pageIndex);
      }
    }
  }
}
</script>

<style scoped>
.page-switching {
  display: flex;
  font-size: 80%;
}
.page-switching .flip-page {
  flex: auto;
  height: 100%;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}
.page-switching .flip-page .page-button {
  aspect-ratio: 1/1;
  height: 80%;
  flex: none;
  background-size: 100% 100%;
  background-repeat: no-repeat;
  background-color: rgba(255,255,255,0.4);
}
.page-switching .flip-page .prev-page {
  background-image: url("../assets/left_arrow.png");
}
.page-switching .flip-page .next-page {
  background-image: url("../assets/right_arrow.png");
}
.page-switching .flip-page .disabled-button {
  cursor: "not-allowed";
  pointer-events: none;
  background-color: rgba(255,255,255,0.2);
}

.page-switching .jump-page {
  flex: none;
  height: 100%;
  align-items: center;
  justify-content: flex-end; 
  display: flex;
}
.page-switching .jump-page input {
  width: 50px;
  background-color: inherit;
  border: 1px solid white;
}
.page-switching .jump-page input:focus {
  color: white;
}
.page-switching .jump-page div {
  margin-left: 10px;
}
</style>
