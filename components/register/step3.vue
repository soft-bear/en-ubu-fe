<template>
  <div class="small">
    <div class="row">
      <div class="col-12">
        <div class="card border-eng">
          <div class="card-header bg-eng text-white">ข้อมูลรายได้ของนักศึกษา (ต่อเดือน)</div>
          <div class="card-body">
            <div class="row">
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="income_father">บิดา</label>
                  <input class="form-control form-control-sm" name="income_father" id="income_father" v-model="form.student_incomes.father" required min="0" type="number" value="0" />
                </div>
              </div>
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="income_mother">มารดา</label>
                  <input class="form-control form-control-sm" name="income_mother" id="income_mother" v-model="form.student_incomes.mother" required min="0" type="number" value="0" />
                </div>
              </div>
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="income_parent">ผู้ปกครอง</label>
                  <input class="form-control form-control-sm" name="income_parent" id="income_parent" v-model="form.student_incomes.parent" required min="0" type="number" value="0" />
                </div>
              </div>
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="income_scholarship">ทุนการศึกษา</label>
                  <input class="form-control form-control-sm" name="income_scholarship" id="income_scholarship" v-model="form.student_incomes.scholarship" required min="0" type="number" value="0" />
                </div>
              </div>
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="income_student_loan">กู้ยืม กยศ.,กรอ.</label>
                  <input class="form-control form-control-sm" name="income_student_loan" id="income_student_loan" v-model="form.student_incomes.studentloan" required min="0" type="number" value="0" />
                </div>
              </div>
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="income_work">ทำงานพิเศษ/ออนไลน์</label>
                  <input class="form-control form-control-sm" name="income_work" id="income_work" v-model="form.student_incomes.parttimejob" required min="0" type="number" value="0" />
                </div>
              </div>
            </div>
            <div class="row">
              <div class="col"><strong>รายได้รวม: <span class="text-success">{{ sumIncome }}</span> บาท</strong></div>
            </div>
          </div>
        </div>
        <div class="card border-eng mt-2">
          <div class="card-header bg-eng text-white">ข้อมูลรายจ่ายของนักศึกษา (ต่อเดือน)</div>
          <div class="card-body">
            <div class="row">
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="expense_monthly">ค่าใช้จ่ายประจำเดือน</label>
                  <input class="form-control form-control-sm" name="expense_monthly" id="expense_monthly" v-model="form.student_expenses.monthly" required min="0" type="number" value="0" />
                </div>
              </div>
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="expense_education">ค่าใช้จ่ายในการศึกษา</label>
                  <input class="form-control form-control-sm" name="expense_education" id="expense_education" v-model="form.student_expenses.education" required min="0" type="number" value="0" />
                </div>
              </div>
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="expense_dorm">ค่าหอพัก</label>
                  <input class="form-control form-control-sm" name="expense_dorm" id="expense_dorm" v-model="form.student_expenses.dorm_rent" required min="0" type="number" value="0"
                  />
                </div>
              </div>
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="expense_travel">ค่าพาหนะ/เดินทาง</label>
                  <input class="form-control form-control-sm" name="expense_travel" id="expense_travel" v-model="form.student_expenses.transport" required min="0" type="number" value="0" />
                </div>
              </div>
              <div class="mb-3 col-md-4">
                <div>
                  <label class="form-label" for="expense_etc">ค่าใช้จ่ายอื่น ๆ </label>
                  <input class="form-control form-control-sm" name="expense_etc" id="expense_etc" v-model="form.student_expenses.etc" required min="0" type="number" value="0" />
                </div>
              </div>
            </div>
            <div class="row">
              <div class="col"><strong>รายจ่ายรวม: <span class="text-danger">{{ sumExpense }}</span> บาท</strong></div>
            </div>
          </div>
        </div>
        <div class="card border-eng mt-2">
          <div class="card-header bg-eng text-white">สถานะภาพบิดา-มารดา</div>
          <div class="card-body">
            <div class="row">
              <div class="col-12 mb-3">
                <select id="status-select" name="status" class="form-control form-control-sm" required v-model="form.parent_relation_status">
                  <option value="">== เลือก ==</option>
                  <option value="2">อยู่ด้วยกัน</option>
                  <option value="3">หย่าขาดกัน</option>
                  <option value="4">แยกทางกัน</option>
                  <option value="5">บิดาเสียชีวิต</option>
                  <option value="6">มารดาเสียชีวิต</option>
                  <option value="7">บิดาและมารดาพิการ</option>
                  <option value="8">บิดาหรือมารดาพิการ</option>
                  <option value="9">กำพร้าทั้งบิดาและมารดา</option>
                  <option value="10">กำพร้าบิดาหรือมารดา</option>
                  <option value="11">บิดาหรือมารดาหย่าร้างแต่ให้การอุปการะทั้งคู่</option>
                  <option value="12">บิดาหรือมารดาหย่าร้างแต่ให้การอุปการะคนเดียว</option>
                  <option value="1">อื่น ๆ</option>
                </select>
              </div>
            </div>
            <div class="row" v-if="form.parent_relation_status == 1">
              <div class="col-12 mb-3">
                <label for="parent_status_etc" class="form-check-label">ระบุสถานะภาพของบิดา-มารดา</label>
                <input type="text" class="form-control form-control-sm" id="parent_status_etc" v-model="form.parent_relation_ect" />
              </div>
            </div>
          </div>
        </div>

        <div class="card border-eng mt-2">
          <div class="card-header bg-eng text-white">ข้อมูลความเป็นอยู่</div>
          <div class="card-body">
            <div class="card border-secondary">
              <div class="card-header bg-secondary text-white">บ้านที่อยู่อาศัย</div>
              <div class="card-body">
                <div class="mb-2">
                  <div class="form-check form-check-inline">
                    <b-form-checkbox name="home_is_owner" type="checkbox" id="checkbox-home-1" class="form-check-input" :value="true" unchecked-value="" v-model="form.residential.is_owner" />
                    <label for="checkbox-home-1" class="form-check-label">เป็นของตนเอง</label>
                  </div>
                </div>
                <div class="mb-2">
                  <div class="form-check form-check-inline">
                    <b-form-checkbox name="home_is_rent" type="checkbox" id="checkbox-home-3" class="form-check-input" :value="true" unchecked-value="" v-model="form.residential.is_rent" />
                    <label for="checkbox-home-3" class="form-check-label" >บ้านเช่า (บาท:เดือน)</label>
                  </div>
                  <input name="home_rent_fee" type="number" :disabled="!form.residential.is_rent" class="form-control form-control-sm" value="0" v-model="form.residential.rent_val" />
                </div>
                <div class="mb-2">
                  <div class="form-check form-check-inline">
                    <b-form-checkbox name="home_is_stay" type="checkbox" id="checkbox-home-2" class="form-check-input" :value="true" unchecked-value="" v-model="form.residential.is_stay" />
                    <label for="checkbox-home-2" class="form-check-label">อาศัยคนอื่น (ระบุ)</label>
                  </div>
                  <input name="home_stay_name" :disabled="!form.residential.is_stay" class="form-control form-control-sm" v-model="form.residential.stay_name" />
                </div>
              </div>
            </div>
            <div class="card border-secondary mt-2">
              <div class="card-header bg-secondary text-white">ที่ดิน</div>
              <div class="card-body">
                <div class="row">
                  <div class="mb-2 col-md-12">
                    <div class="form-check form-check-inline">
                      <b-form-checkbox name="land_is_owner" type="checkbox" id="checkbox-land-1" class="form-check-input" :value="true" unchecked-value="" v-model="form.land_ownership.is_owner" />
                      <label for="checkbox-land-1" class="form-check-label">เป็นของตนเอง (ไร่)</label>
                    </div>
                    <div>
                        <input name="land_owner_val" :disabled="!form.land_ownership.is_owner" v-model="form.land_ownership.own_val" min="0" step="any" type="number" class="form-control form-control-sm" />
                    </div>
                  </div>
                  <div class="mb-2 col-md-12">
                    <div class="form-check form-check-inline">
                      <b-form-checkbox name="land_is_stay" type="checkbox" id="checkbox-land-2" class="form-check-input" :value="true" unchecked-value="" v-model="form.land_ownership.is_stay" />
                      <label for="checkbox-land-2" class="form-check-label">อาศัยคนอื่น (ระบุ)</label>
                    </div>
                    <div>
                      <input name="land_stay_name" :disabled="!form.land_ownership.is_stay" v-model="form.land_ownership.stay_name" class="form-control form-control-sm" value="-" />
                    </div>
                  </div>
                  <div class="mb-2 col-md-6">
                    <div class="form-check form-check-inline">
                      <b-form-checkbox name="land_is_rent" type="checkbox" id="checkbox-land-3" class="form-check-input" :value="true" unchecked-value="" v-model="form.land_ownership.is_rent" />
                      <label for="checkbox-land-3" class="form-check-label">เช่าคนอื่น (ไร่)</label>
                    </div>
                    <div>
                      <input name="land_rent_val" min="0" step="any" :disabled="!form.land_ownership.is_rent" v-model="form.land_ownership.rent_val" type="number" class="form-control form-control-sm" value="0" />
                    </div>
                  </div>
                  <div class="mb-2 col-md-6">
                    <div>
                      <label class="form-check-label">อัตราค่าเช่า (บาท:เดือน)</label>
                      <input name="land_rent_fee" min="0" step="any" :disabled="!form.land_ownership.is_rent" v-model="form.land_ownership.rent_rate" type="number" class="form-control form-control-sm" value="0" />
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="row mt-2" v-if="Object.values(errors).length">
      <div class="col">
        <div class="alert alert-danger">
          <ul class="list-unstyled m-0">
            <li v-for="(error, index) in Object.values(errors)" :key="index">
              <ul class="list-unstyled">
                <li v-for="(sub, index) in error" :key="index">{{ sub }}</li>
              </ul>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div class="row mt-4">
      <div class="col text-center">
        <button class="btn btn-sm btn-outline-eng" type="submit" :disabled="loading" @click="handleSubmit">ดำเนินการต่อไป &gt;&gt;</button>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from 'vuex'

export default {
  props: ['prevData'],
  data() {
    return {
      loading: false,
      errors: {},
      form: {
        student_incomes: {
          father: 0,
          mother: 0,
          parent: 0,
          scholarship: 0,
          studentloan: 0,
          parttimejob: 0,
        },
        student_expenses: {
          monthly: 0,
          education: 0,
          dorm_rent: 0,
          transport: 0,
          etc: 0,
        },
        parent_relation_status: '',
        parent_relation_ect: '',
        residential: {
          is_rent: '',
          is_stay: '',
          is_owner: '',
          rent_val: 0,
          stay_name: '-',
        },
        land_ownership: {
          is_owner: '',
          own_val: 0,
          is_stay: '',
          stay_name: '-',
          is_rent: '',
          rent_val: 0,
          rent_rate: 0
        }
      }
    }
  },
  watch: {
    homeIsRent: function (next) {
      if (!next) {
        this.form.residential.rent_val = 0
      }
    },
    homeIsStay: function (next) {
      if (!next) {
        this.form.residential.stay_name = '-'
      }
    },
    landIsRent: function (next) {
      if (!next) {
        this.form.land_ownership.rent_val = 0
        this.form.land_ownership.rent_rate = 0
      }
    },
    landIsStay: function (next) {
      if (!next) {
        this.form.land_ownership.stay_name = '-'
      }
    },
    landIsOwner: function (next) {
      if (!next) {
        this.form.land_ownership.own_val = 0
      }
    },
  },
  computed: {
    sumIncome() {
      let sum = Object.values(this.form.student_incomes).reduce((a, b) => parseInt(a) + parseInt(b))
      return isNaN(sum) ? 0 : sum.toLocaleString()
    },
    sumExpense() {
      let sum = Object.values(this.form.student_expenses).reduce((a, b) => parseInt(a) + parseInt(b))
      return isNaN(sum) ? 0 : sum.toLocaleString()
    },
    homeIsRent() {return this.form.residential.is_rent},
    homeIsStay() {return this.form.residential.is_stay},
    landIsRent() {return this.form.land_ownership.is_rent},
    landIsStay() {return this.form.land_ownership.is_stay},
    landIsOwner() {return this.form.land_ownership.is_owner},
  },
  methods: {
    ...mapActions({
      nextStep: 'application/nextStep',
      pushStep: 'application/pushStep',
    }),
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
    async handleSubmit() {
      this.loading = true
      this.errors = {}
      try {
        await this.$axios.post('/scholarships/registration/step3', {submission_id: this.$store.state.application.submissionId, ...this.form})
        this.messageBox('ตรวจสอบข้อมูลสำเร็จ, ดำเนินการขั้นตอนต่อไป')
        .then(() => {
          this.nextStep(this.$store.state.application.curStep)
        })
      } catch (error) {
        const {data:{errors}} = error.response
        this.errors = Object.keys(errors).length ? errors : {}
        this.messageBox('ไม่สามารถดำเนินการขั้นต่อไปได้', true)
      }
      this.loading = false
    }
  },
  mounted() {
    Object.keys(this.form).forEach(key => {
      if (key != 'scholarship_id' && this.prevData[key]) {
        this.form[key] = this.prevData[key]
      }
    })
  }
}
</script>
