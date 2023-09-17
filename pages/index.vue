<template>
  <div class="py-4">
    <div class="container">
      <div
        class="title border-bottom d-flex align-items-center justify-content-between py-2"
      >
        <h5>Task</h5>
        <div class="d-flex align-items-center">
          <input
            type="text"
            class="form-control"
            placeholder="Search"
            v-model="searchQuery"
          />
          <div class="dropdown ms-2">
            <button
              class="btn btn-outline-secondary dropdown-toggle"
              type="button"
              id="categoryDropdown"
              data-bs-toggle="dropdown"
              aria-haspopup="true"
              aria-expanded="false"
            >
              {{ selectedCategory || 'Semua' }}
            </button>
            <div class="dropdown-menu" aria-labelledby="categoryDropdown">
              <button class="dropdown-item" @click="selectedCategory = null">
                Semua
              </button>
              <button
                class="dropdown-item"
                v-for="category in categories"
                :key="category"
                @click="selectedCategory = category"
              >
                {{ category }}
              </button>
            </div>
          </div>
          <div class="d-flex align-item-center justify-content-end w-100">
            <span class="me-2">View As</span>
            <button
              class="btn btn-outline-secondary py-1 px-3"
              @click="isGrid = !isGrid"
            >
              {{ isGrid ? 'Grid' : 'List' }}
            </button>
          </div>
        </div>
      </div>
      <div class="list-task row">
        <CardItem
          v-for="(task, index) in resultQuery"
          :key="`${task.title}-${task.category}-${index}`"
          :task="task"
          :isGrid="isGrid"
          @updateTaskStatus="updateTaskStatus(index, $event)"
        />
      </div>
      <div class="action py-2">
        <a
          v-if="!isCreating"
          href="#"
          class="add-button"
          @click="isCreating = !isCreating"
          >Add Task</a
        >
        <div v-else class="add-card">
          <!-- Inside the .add-card div -->
          <div class="add-card">
            <form @submit.prevent="addTask">
              <div class="card mb-2">
                <div class="card-body d-flex flex-column p-0">
                  <input
                    class="form-control border-0 mb-2"
                    placeholder="Title"
                    type="text"
                    v-model="newTask.title"
                  />
                  <textarea
                    class="form-control border-0 small"
                    placeholder="Description"
                    rows="3"
                    v-model="newTask.description"
                  ></textarea>
                </div>
              </div>
              <div class="button-wrapper d-flex">
                <button class="btn btn-primary me-2" type="submit">Save</button>
                <button
                  class="btn btn-outline-secondary"
                  @click="isCreating = !isCreating"
                >
                  Cancel
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import CardItem from '@/components/Card/CardItem.vue'
export default {
  layout(context) {
    return 'custom'
  },
  components: {
    CardItem,
  },
  data() {
    return {
      searchQuery: '',
      isGrid: true,
      isCreating: false,
      tasks: [
        {
          title: '1. Membaca Buku',
          description: 'Baca buku yang telah disiapkan',
          isDone: false,
          category: 'Harian',
        },
        {
          title: '2. Berolahraga',
          description: 'Lakukan latihan fisik minimal 30 menit',
          isDone: false,
          category: 'Harian',
        },
        {
          title: '3. Membantu Orang Tua',
          description: 'Menawarkan diri untuk membantu orang tua apapun itu',
          isDone: false,
          category: 'Harian',
        },
        {
          title: '1. Mengikuti Kelas Beta',
          description: 'Berusah mengikuti kelas dari awal hingga selesai',
          isDone: false,
          category: 'Mingguan',
        },
        {
          title: '2. Mengerjakan Tugas',
          description: 'Mengerjakn tugas yang diberikan oleh mentor',
          isDone: false,
          category: 'Mingguan',
        },
        {
          title: '3. Liburan',
          description: 'Week end yang dipake untuk merefresh pikiran',
          isDone: false,
          category: 'Mingguan',
        },
      ],
      selectedCategory: 'Harian',
      newTask: {
        // Properti untuk tugas baru
        title: '',
        description: '',
      },
    }
  },

  computed: {
    categories() {
      const uniqueCategories = [
        ...new Set(this.tasks.map((task) => task.category)),
      ]
      return uniqueCategories
    },
    resultQuery() {
      if (this.searchQuery) {
        return this.tasks
          .filter((item) => {
            return this.searchQuery
              .toLowerCase()
              .split(' ')
              .every((v) => item.title.toLowerCase().includes(v))
          })
          .filter((item) =>
            this.selectedCategory
              ? item.category === this.selectedCategory
              : true
          )
      } else {
        return this.tasks.filter((item) =>
          this.selectedCategory ? item.category === this.selectedCategory : true
        )
      }
    },
  },
  methods: {
    // ...
    updateTaskStatus(index, updatedTask) {
      this.$set(this.tasks, index, updatedTask)
    },
    // ...
    addTask() {
      // Validasi bahwa judul tugas tidak boleh kosong
      if (this.newTask.title.trim() === '') {
        return
      }

      // Tambahkan tugas baru ke dalam array tasks
      this.tasks.push({
        title: this.newTask.title,
        description: this.newTask.description,
        isDone: false,
        category: this.selectedCategory,
      })

      // Reset form input
      this.newTask.title = ''
      this.newTask.description = ''

      // Tutup form tambah tugas
      this.isCreating = false
    },
  },
}
</script>
<style></style>
