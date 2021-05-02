<template>
  <div class="dialog" :class="{ 'dialog-hide': !show }">
    <div class="dialog-content">
      <div class="dialog-header">
        <div class="dialog-title">THÔNG TIN NHÂN VIÊN</div>
        <div class="dialog-button-close" @click="closeDialog">&#x2715;</div>
      </div>
      <div class="dialog-body">
        <div class="row">
          <div class="col-3 px-1">
            <div class="input-avatar"></div>
            <div class="has-text-center">
              (Vui lòng chọn ảnh có định dạng .jpg, .jpeg, .png, .gif)
            </div>
          </div>
          <div class="col-9">
            <div class="title space-input">A. THÔNG TIN CHUNG:</div>
            <div class="underline-title"></div>
            <div class="row">
              <div class="col-6 space-input">
                <FieldInput
                  ref="EmployeeCode"
                  label="Mã nhân viên"
                  :important="true"
                  :autoFocusInput="true"
                  :errorMsg="errors.EmployeeCode"
                  v-model="employee.EmployeeCode"
                  @blur="onValidEmployeeCode"
                />
              </div>
              <div class="col-6 space-input">
                <FieldInput
                  label="Họ và tên"
                  :errorMsg="errors.FullName"
                  v-model="employee.FullName"
                  @blur="onValidFullName"
                />
              </div>
              <div class="col-6 space-input">
                <FieldInput
                  label="Ngày sinh"
                  typeInput="date"
                  v-model="dateOfBirthInput"
                />
              </div>
              <div class="col-6 space-input">
                <Combobox
                  label="Giới tính"
                  :option="genders"
                  v-model="employee.Gender"
                />
              </div>
              <div class="col-6 space-input">
                <FieldInput
                  label="Số CMTND/ Căn cước"
                  :important="true"
                  :errorMsg="errors.IdentityNumber"
                  v-model="employee.IdentityNumber"
                  @blur="onValidIdentityNumber"
                />
              </div>
              <div class="col-6 space-input">
                <FieldInput
                  label="Ngày cấp"
                  typeInput="date"
                  v-model="identityDateInput"
                />
              </div>
              <div class="col-6 space-input">
                <FieldInput label="Nơi cấp" v-model="employee.IdentityPlace" />
              </div>
              <div class="col-6 space-input"></div>
              <div class="col-6 space-input">
                <FieldInput
                  label="Email"
                  :important="true"
                  typeInput="email"
                  :errorMsg="errors.Email"
                  v-model="employee.Email"
                  @blur="onValidEmail"
                />
              </div>
              <div class="col-6 space-input">
                <FieldInput
                  label="Số điện thoại"
                  :important="true"
                  :errorMsg="errors.PhoneNumber"
                  v-model="employee.PhoneNumber"
                  @blur="onValidPhoneNumber"
                />
              </div>
            </div>

            <div class="title space-input mt-2">B. THÔNG TIN CÔNG VIỆC:</div>
            <div class="underline-title"></div>

            <div class="row mb-2">
              <div class="col-6 space-input">
                <Combobox
                  label="Vị trí"
                  :option="optionPosition"
                  v-model="employee.PositionId"
                />
              </div>
              <div class="col-6 space-input">
                <Combobox
                  label="Phòng ban"
                  :option="optionDepartment"
                  v-model="employee.DepartmentId"
                />
              </div>
              <div class="col-6 space-input">
                <FieldInput
                  label="Mã số thuế cá nhân"
                  v-model="employee.PersonalTaxCode"
                />
              </div>
              <div class="col-6 space-input">
                <FieldInput label="Mức lương cơ bản" v-model="salaryInput" />
              </div>
              <div class="col-6 space-input">
                <FieldInput
                  label="Ngày gia nhập công ty"
                  typeInput="date"
                  v-model="joinDateInput"
                />
              </div>
              <div class="col-6 space-input">
                <Combobox
                  label="Tình trạng công việc"
                  :option="[{ value: '', text: 'Đang làm việc' }]"
                  v-model="employee.WorkStatus"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="dialog-footer">
        <Button text="Hủy" :color="null" @click="closeDialog" />
        <Button text="Lưu" icon="save" @click="onSaveEmployee" />
      </div>
    </div>
  </div>
</template>

<script>
import FieldInput from "../../components/FieldInput";
import Combobox from "../../components/Combobox";
import Button from "../../components/Button";

export default {
  name: "DialogEmployee",
  components: {
    FieldInput,
    Combobox,
    Button,
  },
  props: {
    /**
     * Xác định trạng thái của dialog.
     * true: hiện
     * false: ẩn
     */
    show: Boolean,

    /**
     * nhân viên đang được chỉnh sửa.
     * Lưu ý: Khi thêm mới: nhân viên rỗng.
     */
    employee: {
      type: Object,
      default: null,
    },

    /**
     * Option trong combobox phòng ban.
     */
    optionDepartment: Array,

    /**
     * Option trong combobox vị trí.
     */
    optionPosition: Array,
  },

  data() {
    return {
      /**
       * Option trong combobox giới tính.
       */
      genders: [
        { value: 0, text: "Nữ" },
        { value: 1, text: "Nam" },
        {
          value: null,
          text: "Không xác định",
        },
      ],

      /**
       * Thông tin lỗi.
       */
      errors: {
        EmployeeCode: "",
        FullName: "",
        IdentityNumber: "",
        Email: "",
        PhoneNumber: "",
      },
    };
  },

  methods: {
    /**
     * Hàm đóng dialog.
     */
    closeDialog() {
      this.errors = {
        EmployeeCode: "",
        FullName: "",
        IdentityNumber: "",
        Email: "",
        PhoneNumber: "",
      };
      this.$emit("onClose");
    },

    /**
     * Hàm gọi khi click vào button lưu.
     */
    onSaveEmployee() {
      let valid = true;
      this.onValidEmployeeCode();
      this.onValidFullName();
      this.onValidIdentityNumber();
      this.onValidEmail();
      this.onValidPhoneNumber();
      for (let err in this.errors) {
        if (this.errors[err]) {
          valid = false;
          break;
        }
      }
      if (valid) {
        this.$emit("onSave");
      }
    },

    /**
     * valid mã nhân viên.
     */
    onValidEmployeeCode() {
      if (this.employee && this.employee.EmployeeCode) {
        this.errors.EmployeeCode = "";
      } else {
        this.errors.EmployeeCode = "Mã nhân viên không được để trống.";
      }
    },

    /**
     * valid họ tên nhân viên.
     */
    onValidFullName() {
      if (this.employee && this.employee.FullName) {
        this.errors.FullName = "";
      } else {
        this.errors.FullName = "Tên nhân viên không được để trống.";
      }
    },

    /**
     * valid CMTND.
     */
    onValidIdentityNumber() {
      if (this.employee && this.employee.IdentityNumber) {
        this.errors.IdentityNumber = "";
      } else {
        this.errors.IdentityNumber = "Số CMTND/ Căn cước không được để trống.";
      }
    },

    /**
     * valid email.
     */
    onValidEmail() {
      if (this.employee && this.employee.Email) {
        if (this.employee.Email.match(/(.+)@(.+)/)) {
          this.errors.Email = "";
        } else {
          this.errors.Email = "Email không hợp lệ.";
        }
      } else {
        this.errors.Email = "Email không được để trống.";
      }
    },

    /**
     * valid số điện thoại.
     */
    onValidPhoneNumber() {
      if (this.employee && this.employee.PhoneNumber) {
        if (this.employee.PhoneNumber.match(/^0\d{9}$/)) {
          this.errors.PhoneNumber = "";
        } else {
          this.errors.PhoneNumber = "Số điện thoại không hợp lệ.";
        }
      } else {
        this.errors.PhoneNumber = "Số điện thoại không được để trống.";
      }
    },

    /**
     * Hàm chuyển date string về dạng YYYY-MM-DD
     */
    formatDate(dateStr) {
      if (dateStr) {
        let date = new Date(dateStr);
        let dateString =
          date.getDate() < 10
            ? "0" + date.getDate().toString()
            : date.getDate().toString();
        let monthString =
          date.getMonth() < 9
            ? "0" + (date.getMonth() + 1).toString()
            : (date.getMonth() + 1).toString();
        let yearString = date.getFullYear().toString();
        return `${yearString}-${monthString}-${dateString}`;
      }
      return null;
    },
  },

  computed: {
    /**
     * Computed ngày sinh nhân viên.
     */
    dateOfBirthInput: {
      get() {
        return this.formatDate(this.employee.DateOfBirth);
      },
      set(val) {
        this.employee.DateOfBirth = val;
      },
    },

    /**
     * computed ngày cấp CMTND.
     */
    identityDateInput: {
      get() {
        return this.formatDate(this.employee.IdentityDate);
      },
      set(val) {
        this.employee.IdentityNumber = val;
      },
    },

    /**
     * Computed lương của nhân viên. Định dạng kiểu xxx.xxx.xxx
     */
    salaryInput: {
      get() {
        if (this.employee && this.employee.Salary) {
          return new Intl.NumberFormat("vi-VN").format(this.employee.Salary);
        }
        return this.salary;
      },
      set(val) {
        let num = parseFloat(val.replace(/(\D+)/g, ""));
        this.employee.Salary = num;
      },
    },

    /**
     * Computed ngày tham gia công ty của nhân viên.
     */
    joinDateInput: {
      get() {
        return this.formatDate(this.employee.JoinDate);
      },
      set(val) {
        this.employee.JoinDate = val;
      },
    },
  },

  watch: {
    show: function(val){
      if(val){
        this.$nextTick(function(){
          this.$refs.EmployeeCode.$el.children[1].focus();
        });
      }
    }
  }
};
</script>

<style scoped>
.dialog {
  overflow: auto;
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: #000;
  background-color: rgba(0, 0, 0, 0.4);
}

.dialog.dialog-hide {
  display: none;
}

.dialog .dialog-content {
  width: 900px;
  margin: 50px auto;
  background-color: #fff;
  position: relative;
}

.dialog .dialog-header {
  padding: 16px;
}

.dialog .dialog-header .dialog-title {
  font-size: 15px;
  font-family: Google-Sans-Bold;
}

.dialog .dialog-header .dialog-button-close {
  font-size: 16px;
  position: absolute;
  right: 0;
  top: 0;
  height: 40px;
  width: 40px;
  line-height: 40px;
  text-align: center;
  cursor: pointer;
  border-bottom-left-radius: 4px;
}

.dialog .dialog-header .dialog-button-close:hover {
  background-color: #e5e5e5;
}

.dialog .dialog-body {
  padding-left: 24px;
  padding-right: 24px;
}

.dialog .dialog-footer {
  padding-right: 24px;
  height: 60px;
  background-color: #e5e5e5;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

/* nội dung dialog thêm và sửa */

.dialog .dialog-body .input-avatar {
  margin-top: 16px;
  background-image: url("../../assets/img/default-avatar.jpg");
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  width: 100%;
  height: 180px;
  border-radius: 50%;
  border: 1px solid #bbb;
}

.dialog .dialog-body .space-input {
  padding-left: 4px;
  padding-right: 4px;
  padding-top: 4px;
}

.dialog .dialog-body .title {
  font-size: 15px;
  margin-bottom: 4px;
}

.dialog .dialog-body .underline-title {
  border-top: 5px solid #019160;
  width: 100px;
  margin-left: 4px;
}
</style>