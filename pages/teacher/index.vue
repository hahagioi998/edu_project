<template>
  <div id="aCoursesList" class="bg-fa of">
    <!-- 讲师列表 开始 -->
    <section class="container">
      <section class="c-sort-box unBr">
        <div>
          <!-- /无数据提示 开始-->
          <section class="no-data-wrap" v-if="data.total==0">
              <em class="icon30 no-data-ico">&nbsp;</em>
              <span class="c-666 fsize14 ml10 vam">没有相关数据，小编正在努力整理中...</span>
          </section>
          <!-- /无数据提示 结束-->

          <!-- /数据列表 开始-->
          <article v-if="data.total>0" class="i-teacher-list">
              <ul class="of">
                  <li v-for="item in data.items" :key="item.id">
                      <section class="i-teach-wrap">
                          <div class="i-teach-pic">
                              <a :href="'/teacher/'+item.id" :title="item.name">
                                  <img :src="item.avatar" :alt="item.name" height="142" hright="142">
                              </a>
                          </div>
                          <div class="mt10 hLh30 txtOf tac">
                              <a :href="'/teacher/'+item.id" :title="item.name" class="fsize18 c-666">{{ item.name }}</a>
                          </div>
                          <div class="hLh30 txtOf tac">
                              <span class="fsize14 c-999" >{{ item.career }}</span>
                          </div>
                          <div class="mt15 i-q-txt">
                              <p class="c-999 f-fA">{{ item.intro }}</p>
                          </div>
                      </section>
                  </li>
              </ul>
              <div class="clear"/>
          </article>
          <!-- /数据列表 结束-->
        </div>


        <!-- 公共分页 开始 -->
        <div>
          <div class="paging">
            <!-- undisable这个class是否存在，取决于数据属性hasPrevious -->
            <a
              :class="{undisable: !data.hasPrevious}"
              href="#"
              title="首页"
              @click.prevent="gotoPage(1)">首</a>
            <a
              :class="{undisable: !data.hasPrevious}"
              href="#"
              title="前一页"
              @click.prevent="gotoPage(data.current-1)">&lt;</a>
            <a
              v-for="page in data.pages"
              :key="page"
              :class="{current: data.current == page, undisable: data.current == page}"
              :title="'第'+page+'页'"
              href="#"
              @click.prevent="gotoPage(page)">{{ page }}</a>
            <a
              :class="{undisable: !data.hasNext}"
              href="#"
              title="前一页"
              @click.prevent="gotoPage(data.current+1)">&gt;</a>
            <a
              :class="{undisable: !data.hasNext}"
              href="#"
              title="末页"
              @click.prevent="gotoPage(data.pages)">末</a>
            <div class="clear"/>
          </div>
        </div>
        <!-- 公共分页 结束 -->
      </section>
    </section>
    <!-- /讲师列表 结束 -->
  </div>
</template>

<script>
import teacher from "@/api/teacher"

//设置默认的页面显示记录数
var limit = 8
export default {

  asyncData({ params, error }) {
    console.log(params)
    return teacher.getPageList(1, limit).then(response => {
      console.log(response.data.data);
      return { data: response.data.data }
    });
  },
  methods: {
    gotoPage(page){
     //查询时页数不能小于1 和大于总页数
      if(page>=1 && page<=this.data.pages)
      teacher.getPageList(page, limit).then(response => {
          this.data = response.data.data
      })
    }
  }
};
</script>