<template>
  <div class="el-form-item">
    <el-input v-model.trim="keyword" placeholder="请输入关键词">
      <template #append>
        <el-button icon="el-icon-search" @click="searchCourse()"/>
      </template>
    </el-input>
  </div>
  <el-row v-if="courses && courses.length > 0" :gutter="20">
    <el-col v-for="course in courses" :xs="24" :sm="6" class="course-card">
      <router-link :to="{ name: 'Course-Content', params: { id: course.id }}">
        <el-card :body-style="{ 'padding': '0px', 'min-height': '275px' }">
          <el-image :src="course.coverPicture" class="card-cover-picture"/>
          <div class="card-text">
            <div class="card-course-name">
              {{ course.name }}
            </div>
          </div>
        </el-card>
      </router-link>
    </el-col>
  </el-row>
  <el-empty v-else/>
  <div class="pagination">
    <el-pagination background layout="prev, pager, next" :pager-count="5" :total="size" :page-size="20"
                   :hide-on-single-page="true" @current-change="handlePageChange">
    </el-pagination>
  </div>
</template>

<script>
import {searchCourse} from '../utils/api'

export default {
  name: 'Course-Search',
  data() {
    return {
      keyword: '',
      courses: {},
      size: 0,
      pageNum: 1
    }
  },
  methods: {
    searchCourse() {
      if (this.keyword === '') {
        return
      }
      searchCourse({keyword: this.keyword, pageNum: this.pageNum}).then(result => {
        if (result.code === '0000') {
          this.courses = result.data.list
          this.size = result.data.size
        }
      })
    },
    handlePageChange(pageNum) {
      this.pageNum = pageNum
      this.searchCourse()
    }
  }
}
</script>