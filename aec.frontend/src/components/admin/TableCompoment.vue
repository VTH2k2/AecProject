<script lang="ts" setup>
import { ref, unref, reactive } from 'vue'
import { Search, CirclePlus, Delete, MoreFilled, Edit } from '@element-plus/icons-vue'
import { ElNotification, ElMessageBox } from 'element-plus'
import { useRouter } from 'vue-router'

const router = useRouter()
const input2 = ref('')

interface User {
  date: string
  name: string
  address: string
}
const tableData: User[] = [
  {
    date: '2016-05-04',
    name: 'Aleyna Kutzner',
    address: 'Lohrbergstr. 86c, Süd Lilli, Saarland',
  },
  {
    date: '2016-05-03',
    name: 'Helen Jacobi',
    address: '760 A Street, South Frankfield, Illinois',
  },
  {
    date: '2016-05-02',
    name: 'Brandon Deckert',
    address: 'Arnold-Ohletz-Str. 41a, Alt Malinascheid, Thüringen',
  },
  {
    date: '2016-05-01',
    name: 'Margie Smith',
    address: '23618 Windsor Drive, West Ricardoview, Idaho',
  },
]

const handleEdit = (row) => {
  console.log('Sửa:', row)
  // Thực hiện logic sửa ở đây
}

const emit = defineEmits<{
  (e: 'show-alert', payload: { type: string; title: string; description: string }): void
}>()

const handleDelete = (row) => {
  ElMessageBox.confirm('Bạn có chắc muốn xóa bản ghi này?', 'Thông báo', {
    confirmButtonText: 'Có',
    cancelButtonText: 'Quay lại',
    type: 'warning',
  })
    .then(() => {
      ElNotification({
        title: 'Success',
        message: 'Delete completed',
        type: 'success',
        position: 'top-right', // 👈 Vị trí góc trên phải
        duration: 3000,
        customClass: 'custom-success-notification',
      })
    })
    .catch(() => {
      ElNotification({
        title: 'Cancelled',
        message: 'Delete canceled',
        type: 'info',
        position: 'top-right', // 👈 Vị trí góc trên phải
        duration: 3000,
        customClass: 'custom-secondary-notification',
      })
    })
}

//Popover
const buttonRef = ref()
const popoverRef = ref()
const onClickOutside = () => {
  unref(popoverRef).popperRef?.delayHide?.()
}

// Handle Create
const openForm = () => {
  router.push('/form')
}
</script>

<template>
  <div class="w-full">
    <el-card class="">
      <div class="w-full justify-between items-center flex mb-4 px-2">
        <div>
          <el-input
            v-model="input2"
            style="width: 240px"
            placeholder="Tìm kiếm"
            :suffix-icon="Search"
          />
        </div>
        <div>
          <el-button type="danger" :icon="Delete" plain>Xóa đã chọn</el-button>
          <el-button
            class="!border-teal-500 hover:!bg-teal-500 !text-teal-500 hover:!text-white"
            type="success"
            :icon="CirclePlus"
            plain
            @click="openForm"
            >Thêm mới</el-button
          >
        </div>
      </div>
      <div>
        <el-table :data="tableData" style="min-width: 100%">
          <el-table-column type="selection" width="55" />
          <el-table-column label="Date" width="120">
            <template #default="scope">{{ scope.row.date }}</template>
          </el-table-column>
          <el-table-column property="name" label="Name" width="120" />
          <el-table-column
            property="address"
            label="use show-overflow-tooltip"
            width="240"
            show-overflow-tooltip
          />
          <el-table-column property="address" label="address" />
          <el-table-column fixed="right" label="Tác vụ" min-width="10">
            <template #default="scope">
              <el-popover placement="bottom-start" trigger="click">
                <div class="flex flex-col items-start space-y-2">
                  <el-button
                    class="w-full !m-0 !justify-start"
                    text
                    @click="handleEdit(scope.row)"
                    :icon="Edit"
                    >Sửa</el-button
                  >
                  <el-button
                    class="w-full !m-0 !justify-start"
                    text
                    @click="handleDelete(scope.row)"
                    :icon="Delete"
                    >Xoá</el-button
                  >
                </div>
                <template #reference>
                  <el-button type="secondary" size="small">
                    <el-icon><MoreFilled /></el-icon>
                  </el-button>
                </template>
              </el-popover>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-card>
  </div>
</template>

<style>
@import url('../../assets/custom_notification.css');
</style>
