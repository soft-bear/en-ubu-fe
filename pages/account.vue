<template>
  <div class="container">
    <div class="row my-4">
      <div class="col">
        <div class="row">
          <div class="col">
            <h4><strong>บัญชีผู้ใช้</strong></h4>
          </div>
        </div>
        <div class="row mt-2">
          <div class="col">
            <b-card no-body border-variant="primary" header-bg-variant="primary" header-text-variant="white">
              <template #header>
                <div v-b-toggle.student-info>ข้อมูลนักศึกษา</div>
              </template>
              <b-collapse id="student-info" visible>
                <div class="card-body small">
                  <b-form @submit.stop.prevent="setStudentData">
                    <div class="row">
                      <div class="col-12 col-md-4">
                        <div class="form-group">
                          <label class="form-label" for="title">คำนำหน้านาม</label>
                          <b-select :options="titleOptions" v-model="student.title" size="sm" id="title" required>
                          </b-select>
                        </div>
                      </div>
                      <div class="col-12 col-md-8">
                        <div class="form-group">
                          <label class="form-label" for="fullname">ชื่อจริง</label>
                          <input type="text" class="form-control form-control-sm" id="fullname" v-model="student.name" />
                        </div>
                      </div>
                      <div class="col-12 col-md-4">
                        <div class="form-group">
                          <label class="form-label" for="student_id">รหัสนักศึกษา</label>
                          <input type="number" class="form-control form-control-sm" id="student_id" v-model="student.student_id" min="0" />
                        </div>
                      </div>
                      <div class="col-12 col-md-4">
                        <div class="form-group">
                          <label class="form-label" for="faculty_id">คณะ/วิทยาลัย</label>
                          <b-select size="sm" :options="facultyOptions" id="faculty_id" v-model="student.faculty_id" @change="student.department_id = ''"></b-select>
                        </div>
                      </div>
                      <div class="col-12 col-md-4">
                        <div class="form-group">
                          <label class="form-label" for="department_id">ภาควิชา</label>
                          <b-select size="sm" :options="departmentOptions" id="department_id" v-model="student.department_id"></b-select>
                        </div>
                      </div>
                      <div class="col-12">
                        <div class="form-group">
                            <label class="form-label" for="student-photo">ภาพนักศึกษา<span v-if="this.$auth.user.data.student?.student_photo" class="text-success">&nbsp;(อัปโหลดแล้ว)&nbsp;</span><small class="text-danger">ประเภท jpg, jpeg, png และไม่เกิน 10MB</small></label>
                            <b-form-file id="student-photo" @change="handleStudentPhotoUpload" size="sm" :required="this.$auth.user.data.student ? false : true"></b-form-file>
                        </div>
                      </div>
                      <div class="col-12">
                        <div class="form-group">
                          <label class="form-label">สิทธิ์การใช้งาน</label>
                          <input type="text" class="form-control form-control-sm" disabled :value="$auth.user.data.level" />
                        </div>
                      </div>
                    </div>
                    <div class="row">
                      <div class="col">
                        <button type="submit" class="btn btn-primary btn-sm" :disabled="loading">บันทึกการเปลี่ยนแปลง</button>
                      </div>
                    </div>
                  </b-form>
                </div>
              </b-collapse>
            </b-card>
          </div>
        </div>
        <div class="row mt-4">
          <div class="col">
            <b-card no-body border-variant="secondary" header-bg-variant="secondary" header-text-variant="white">
              <template #header>
                <div v-b-toggle.account-password>เปลี่ยนรหัสผ่าน</div>
              </template>
              <b-collapse id="account-password">
                <div class="card-body">
                  <b-form @submit.stop.prevent="changePassword">
                    <div class="row">
                      <div class="col small">
                        <div class="mb-3">
                          <label class="form-label" for="old_password">รหัสผ่านเดิม</label>
                          <input type="password" class="form-control form-control-sm" v-model="password.old_password"
                            id="old_password" required>
                        </div>
                        <div class="mb-3">
                          <label class="form-label" for="set_password">รหัสผ่านใหม่</label>
                          <input type="password" class="form-control form-control-sm" v-model="password.set_password"
                            id="set_password" required>
                        </div>
                        <div class="mb-3">
                          <label class="form-label" for="con_password">ยืนยันรหัสผ่าน</label>
                          <input type="password" class="form-control form-control-sm" v-model="password.con_password"
                            id="con_password" required>
                        </div>
                      </div>
                    </div>
                    <div class="row">
                      <div class="col">
                        <button type="submit" class="btn btn-primary btn-sm" :disabled="loading">เปลี่ยนรหัสผ่าน</button>
                      </div>
                    </div>
                  </b-form>
                </div>
              </b-collapse>
            </b-card>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  middleware: 'auth',
  data() {
    return {
      loading: false,
      student: {
        student_id: '',
        title: '',
        name: '',
        faculty_id: '',
        department_id: '',
        student_photo: '',
      },
      password: {
        old_password: '',
        set_password: '',
        con_password: '',
      },
      titleOptions: [
        { text: '== เลือก ==', value: '' },
        { text: 'นาง', value: 'นาง' },
        { text: 'นางสาว', value: 'นางสาว' },
        { text: 'นาย', value: 'นาย' }
      ],
      faculties: []
    }
  },
  computed: {
    facultyOptions() {
      const options = [{ text: '== เลือก ==', value: '' }]
      const result = this.faculties.map(i => ({ text: i.name, value: i.id }))

      return [...options, ...result]
    },
    departmentOptions() {
      const options = [{ text: '== เลือก ==', value: '' }]
      const faculty = this.student.faculty_id != '' ? this.faculties.find(i => i.id == this.student.faculty_id) : []
      const getData = faculty != null && Object.keys(faculty).length ? faculty.departments.map(i => ({ text: i.name, value: i.id })) : []
      const result = getData.sort((a,b) => a.text.localeCompare(b.text, "th"))

      return [...options, ...result]
    }
  },
  methods: {
    messageBox(data, error = false) {
      return this.$bvModal.msgBoxOk(data, {
        title: !error ? 'สำเร็จ' : 'ผิดพลาด',
        size: 'sm',
        buttonSize: 'sm',
        okVariant: !error ? 'success' : 'danger',
        headerClass: 'p-2 border-bottom-0',
        footerClass: 'p-2 border-top-0',
        centered: true
      })
    },
    async setStudentData() {
      this.loading = true
      try {
        await this.$axios.put('/user/me/student', this.student)
        this.messageBox('แก้ไขข้อมูลนักศึกษาแล้ว')
      } catch (e) {
        this.messageBox('ไม่สามารถแก้ไขข้อมูลนักศึกษา', true)
      }
      this.loading = false
    },
    async changePassword() {
      this.loading = true
      try {
        await this.$axios.put('/user/me/change-password', this.password)
        this.password = {
          old_password: '',
          set_password: '',
          con_password: '',
        }
        this.messageBox('เปลี่ยนรหัสผ่านสำเร็จแล้ว')
      } catch (e) {
        this.messageBox('ไม่สามารถเปลี่ยนรหัสผ่าน', true)
      }
      this.loading = false
    },
    async setStudentData() {
      this.loading = true
      try {
        const payload = new FormData()
        payload.append('_method', 'PUT')

        Object.keys(this.student).forEach(item => {
          if (item == 'student_photo' && !this.student['student_photo']) return
          payload.append(item, this.student[item])
        })

        await this.$axios.post('/user/me/student', payload, { headers: { 'Content-Type': 'multipart/form-data' } })
        this.student.student_photo = ''
        this.messageBox('บันทึกการเปลี่ยนแปลงสำเร็จแล้ว')
      } catch (e) {
        this.messageBox('ไม่สามารถบันทึกการเปลี่ยนแปลง', true)
      }
      this.loading = false
    },
    async getFacultiesData() {
      try {
        const { data: { data } } = await this.$axios.get('/faculties')
        this.faculties = data
      } catch (e) { }
    },
    handleStudentPhotoUpload(event) {
			this.student.student_photo = event.target.files[0] ?? ''
		}
  },
  async created() {
    this.loading = true
    this.student = {
      student_id: this.$auth.user.data.student?.student_id ?? '',
      title: this.$auth.user.data.student?.title ?? '',
      name: this.$auth.user.data.name,
      faculty_id: this.$auth.user.data.student?.faculty_id ?? '',
      department_id: this.$auth.user.data.student?.department_id ?? '',
    },
    await this.getFacultiesData()
    this.loading = false
  }
}
</script>
