<template>
  <div class="container py-4" v-if="customer">
    <div class="card border-danger shadow shadow-sm" style="max-width: 480px; margin: 0 auto">
      <div class="card-header fw-semibold">CẢNH BÁO</div>
      <div class="card-body">
        <h5 class="card-title fw-semibold">XÓA TÀI KHOẢN KHÁCH HÀNG</h5>
        <hr>
        <p class="card-text">Bạn có chắc chắn muốn xóa khách hàng dưới đây không?</p>
        <form>
          <div class="mb-3">
            <label class="form-label">Họ và tên</label>
            <input type="text" :value="customer.ten" class="form-control" readonly />
          </div>

          <div class="mb-3">
            <label class="form-label">Số điện thoại</label>
            <input type="text" :value="customer.sdt" class="form-control" readonly />
          </div>

          <div class="mb-3">
            <label class="form-label">Địa chỉ</label>
            <input type="text" :value="customer.diachi" class="form-control" readonly />
          </div>
        </form>
        <p class="card-text"><i class="fa-solid fa-circle-exclamation" style="color: yellow;"></i> LƯU Ý: Bạn sẽ không thể hoàn tác lại quá trình này sau khi ấn nút xác nhận xóa.</p>
        <button class="btn btn-danger" @click="handleDelete">
          <span><i class="fa-solid fa-trash"></i> Tôi đã hiểu và xác nhận xóa </span>
        </button>

        <RouterLink to="/admin/customers" class="btn"><i class="fa-solid fa-arrow-rotate-left"></i> Quay lại</RouterLink>
      </div>
    </div>
  </div>
</template>

<script>
import khachhangService from '@/services/khachhang.service'
import { ref } from 'vue'

export default {
  name: 'AdminDeleteCustomer',
  data() {
    const customer = ref(null)

    return { customer }
  },
  methods: {
    async handleDelete() {
      try {
        await khachhangService.deleteById(this.$route.params.id)

        this.$toast.info('Xóa khách hàng thành công')
        this.$router.back()
      } catch (error) {
        this.$toast.error(error.message)
      }
    }
  },
  async beforeMount() {
    this.customer = (await khachhangService.getById(this.$route.params.id)).khach
  }
}
</script>
