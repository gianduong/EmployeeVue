<template>
  <div>
    <div class="content">
      <div class="title-box">
        <div class="title">Danh sách nhân viên</div>
        <Button
          text="Thêm nhân viên"
          iconLeft="user-plus"
          @click="btnClickAddEmployee"
        />
      </div>
      <div class="toolbar-box mt-1">
        <div class="toolbar-left">
          <FieldInput
            styleInput="width: 280px"
            placeholder="Tìm kiếm theo Mã, Tên hoặc Số điện thoại"
            icon="search"
            v-model="employeeFilter"
            @change="onHandleEmployeeFilter"
          />

          <Combobox
            styleCombobox="width: 150px"
            class="ml-1"
            :option="optionDepartment"
            v-model="selectedDepartment"
          />

          <Combobox
            styleCombobox="width: 150px"
            class="ml-1"
            :option="optionPosition"
            v-model="selectedPosition"
          />
        </div>

        <div class="toolbar-right">
          <Button :onlyIcon="true" icon="sync-alt" @click="btnRefreshData" />
          <Button
            :onlyIcon="true"
            icon="trash"
            styleBtn="background-color: #f20; color: #fff; margin-left: 8px"
            @click="btnDelEmployee"
          />
        </div>
      </div>
      <div class="data">
        <table v-if="employees && employees.length > 0" class="table">
          <thead>
            <tr>
              <th>Mã nhân viên</th>
              <th>Họ và tên</th>
              <th>Giới tính</th>
              <th>Ngày sinh</th>
              <th>Điện thoại</th>
              <th>Email</th>
              <th>Chức vụ</th>
              <th>Phòng ban</th>
              <th class="has-text-right">Mức lương cơ bản</th>
              <th>Tình trạng công việc</th>
            </tr>
          </thead>
          <tbody>
            <EmployeeItem
              v-for="e in employees"
              :key="e.EmployeeId"
              :employee="e"
              :selectedId="selectedEmployeeId"
              @dblclick="onDblClickEmployeeItem"
              @click="onClickEmployeeItem"
            />
          </tbody>
        </table>

        <div v-else>
          <div
            class="has-text-center"
            style="font-size: 15px; font-weight: bold; margin-top: 16px"
          >
            Không có dữ liệu.
          </div>
        </div>
      </div>

      <div class="footer">
        <div class="footer-left">
          Hiển thị {{ startRecord }}-{{ endRecord }}/{{ totalEmployees }} nhân
          viên
        </div>
        <Pagination :page="page" :totalPage="totalPage" />
        <div class="footer-right">{{ limit }} nhân viên/trang</div>
      </div>
    </div>

    <DialogEmployee
      :show="isShowDialogEmployee"
      :employee="selectedEmployee"
      :optionDepartment="optionDepartment.slice(1)"
      :optionPosition="optionPosition.slice(1)"
      @onClose="onCloseDialogEmployee"
      @onSave="saveEmployee"
    />

    <AlertDialog
      :show="isShowAlertDialog"
      :msg="msg"
      @onClose="onCloseAlertDialog"
    />

    <ConfirmDialog
      :show="isShowConfirmDialog"
      :msg="msgConfirm"
      @onClose="onCloseConfirmDialog"
      @onOk="delEmployee"
    />
  </div>
</template>

<script>
import Button from "../../components/Button";
import FieldInput from "../../components/FieldInput";
import Combobox from "../../components/Combobox";
import Pagination from "../../components/Pagination";

import EmployeeItem from "./EmployeeItem";

import DialogEmployee from "./DialogEmployee";
import AlertDialog from "../../components/AlertDialog";
import ConfirmDialog from "../../components/ConfirmDialog";

import axios from "axios";
export default {
  name: "EmployeeList",
  components: {
    Button,
    FieldInput,
    Combobox,
    Pagination,
    EmployeeItem,
    DialogEmployee,
    AlertDialog,
    ConfirmDialog,
  },
  data() {
    return {
      /**
       * các option cho combobox phòng ban.
       */
      optionDepartment: [{ value: "", text: "Tất cả phòng ban" }],

      /**
       * Các option cho combobox vị trí.
       */
      optionPosition: [{ value: "", text: "Tất cả vị trí" }],

      /**
       * phòng ban đang được chọn tại combobox phòng ban. Mặc định là "".
       */
      selectedDepartment: "",

      /**
       * Vị trí đang được chọn tại combobox vị trí. Mặc định là "".
       */
      selectedPosition: "",

      /**
       * Biến xác định trạng thái của dialog thêm và sửa.
       * true: hiện
       * false: ẩn
       */
      isShowDialogEmployee: false,

      /**
       * Biến xác định trạng thái của dialog thông báo.
       * true: hiện
       * false: ẩn.
       */
      isShowAlertDialog: false,

      /**
       * Biến xác định trạng thái của dialog xác nhận.
       * true: hiện
       * false: ẩn.
       */
      isShowConfirmDialog: false,

      /**
       * Câu thông báo hiển thị trong dialog thông báo. Mặc định là "".
       */
      msg: "",

      /**
       * Câu thông báo hiển thị trong dialog xác nhận. Mặc định là "".
       */
      msgConfirm: "",

      /**
       * Mảng nhân viên được fetch từ server (có filter).
       */
      employees: [],

      /**
       * Dữ liệu nhân viên đang được chọn. Biến này dùng để truyền qua component con dialog thêm và sửa.
       * Mặc định là thêm mới - Nhân viên rỗng.
       */
      selectedEmployee: {
        Salary: null,
        DateOfBirth: null,
        IdentityDate: null,
        JoinDate: null,
        Gender: 0,
      },

      /**
       * Id của nhân viên đang được chọn từ bảng. Id này dùng xác định xóa nhân viên cần xóa.
       * Mặc định là null.
       */
      selectedEmployeeId: null,

      /**
       * Trang hiện tại. Mặc định là 1.
       */
      page: 1,

      /**
       * Số lượng nhân viên trên 1 trang. Mặc định là 10.
       */
      limit: 10,

      /**
       * tổng số trang.
       */
      totalPage: 0,

      /**
       * Tổng số nhân viên.
       */
      totalEmployees: 0,

      /**
       * Filter danh sách nhân viên theo mã hoặc tên hoặc số điện thoại. Mặc định là "".
       */
      employeeFilter: "",
      timeOut: null,
    };
  },
  created() {
    if (this.$route.query && this.$route.query.page) {
      this.page = parseInt(this.$route.query.page);
    } else {
      this.page = 1;
    }
    this.fetchData();
    this.fetchDepartments();
    this.fetchPositions();
  },
  methods: {
    /**
     * Hàm lấy danh sách có lọc nhân viên từ server.
     */
    fetchData() {
      let url = `http://api.manhnv.net/v1/Employees/employeeFilter?pageSize=${this.limit}&pageNumber=${this.page}`;
      if (this.employeeFilter) {
        url += `&employeeFilter=${this.employeeFilter}`;
      }
      if (this.selectedDepartment) {
        url += `&departmentId=${this.selectedDepartment}`;
      }
      if (this.selectedPosition) {
        url += `&positionId=${this.selectedPosition}`;
      }

      axios
        .get(url)
        .then((res) => {
          if (res.status == 200) {
            return res.data;
          }
          return Promise.reject("Không có dữ liệu.");
        })
        .then((data) => {
          this.employees = data.Data;
          this.totalPage = data.TotalPage;
          this.totalEmployees = data.TotalRecord;
        })
        .catch((err) => console.log(err));
    },

    /**
     * Hàm lấy tất cả các phòng ban.
     */
    fetchDepartments() {
      axios
        .get("http://api.manhnv.net/api/Department")
        .then((res) => res.data)
        .then((data) => {
          for (let d of data) {
            let optDepartment = {
              value: d.DepartmentId,
              text: d.DepartmentName,
            };
            this.optionDepartment.push(optDepartment);
          }
        })
        .catch((err) => console.log(err));
    },

    /**
     * Hàm lấy tất cả vị trí từ server.
     */
    fetchPositions() {
      axios
        .get("http://api.manhnv.net/v1/Positions")
        .then((res) => res.data)
        .then((data) => {
          for (let p of data) {
            let optPosition = {
              value: p.PositionId,
              text: p.PositionName,
            };
            this.optionPosition.push(optPosition);
          }
        })
        .catch((err) => console.log(err));
    },

    /**
     * Hàm reset bộ lọc về mặc định.
     */
    resetFilter() {
      this.page = 1;
      this.TotalPage = 0;
      this.totalEmployees = 0;
      this.selectedDepartment = "";
      this.selectedPosition = "";
      this.employeeFilter = "";
    },

    /**
     * Sự kiện click button refresh.
     */
    btnRefreshData() {
      this.resetFilter();
      this.fetchData();
    },

    /**
     * Hàm hiển thị dialog thêm và sửa nhân viên.
     */
    showDialogEmployee() {
      this.isShowDialogEmployee = true;
    },

    onHandleEmployeeFilter() {
      clearTimeout(this.timeOut);
      this.timeOut = setTimeout(() => {
        this.fetchData();
      }, 300);
    },

    /**
     * Hàm click button thêm nhân viên.
     */
    btnClickAddEmployee() {
      axios
        .get("http://api.manhnv.net/v1/Employees/NewEmployeeCode")
        .then((res) => res.data)
        .then((data) => {
          if (data.indexOf("NV") == -1) {
            data = "NV" + data;
          }
          this.selectedEmployee.EmployeeCode = data;
          this.showDialogEmployee();
        })
        .catch((err) => console.log(err));
    },

    /**
     * Hàm đóng dialog thêm và sửa nhân viên.
     */
    onCloseDialogEmployee() {
      this.isShowDialogEmployee = false;
      this.selectedEmployee = {
        Salary: null,
        DateOfBirth: null,
        IdentityDate: null,
        JoinDate: null,
        Gender: 0,
      };
    },

    /**
     * Hàm double click vào employeeItem.
     */
    onDblClickEmployeeItem(employeeId) {
      axios
        .get(`http://api.manhnv.net/v1/employees/${employeeId}`)
        .then((res) => res.data)
        .then((data) => {
          this.selectedEmployee = data;
          this.showDialogEmployee();
        })
        .catch((err) => console.log(err));
    },

    /**
     * Hàm click vào employeeItem.
     */
    onClickEmployeeItem(employeeId) {
      if (this.selectedEmployeeId && this.selectedEmployeeId == employeeId) {
        this.selectedEmployeeId = null;
      } else {
        this.selectedEmployeeId = employeeId;
      }
    },

    /**
     * Hàm đóng dialog thông báo.
     */
    onCloseAlertDialog() {
      this.isShowAlertDialog = false;
      this.msg = "";
    },

    /**
     * Hàm đóng dialog xác nhận.
     */
    onCloseConfirmDialog() {
      this.isShowConfirmDialog = false;
      this.msgConfirm = "";
    },

    /**
     * Hàm show dialog thông báo với msg.
     */
    showAlertDialogWithMsg(msg) {
      this.msg = msg;
      this.isShowAlertDialog = true;
    },

    /**
     * Hàm hiển thị dialog xác nhận với một câu thông báo msg.
     */
    showConfirmDialogWithMsg(msg) {
      this.msgConfirm = msg;
      this.isShowConfirmDialog = true;
    },

    /**
     * lưu thông tin nhân viên.
     */
    saveEmployee() {
      // cấu hình axios req.
      let configAxios = {
        data: this.selectedEmployee,
      };

      if (this.selectedEmployee.EmployeeId) {
        // nếu là sửa nhân viên.
        configAxios.url = `http://api.manhnv.net/v1/employees/${this.selectedEmployee.EmployeeId}`;
        configAxios.method = "PUT";
      } else {
        // nếu là thêm mới nhân viên.
        configAxios.url = "http://api.manhnv.net/v1/employees";
        configAxios.method = "POST";
      }

      // tiến hành call api lưu thông tin nhân viên.
      axios(configAxios)
        .then(() => {
          // thực hiện đóng dialog thêm và sửa nhân viên.
          this.onCloseDialogEmployee();

          // show dialog thông báo với lời thông báo Lưu thành công.
          this.showAlertDialogWithMsg("Lưu thành công.");

          // Lấy lại dữ liệu từ api với bộ lọc mặc định.
          this.btnRefreshData();
        })
        .catch((err) => {
          // show dialog thông báo khi lưu thất bại.
          this.showAlertDialogWithMsg("Lưu thất bại.");
          console.log(err);
        });
    },

    /**
     * Sự kiện click button xóa nhân viên.
     * Lưu ý: phải đã chọn nhân viên cần xóa.
     */
    btnDelEmployee() {
      if (this.selectedEmployeeId) {
        this.showConfirmDialogWithMsg(
          "Bạn có chắc muốn xóa nhân viên này không ?"
        );
      }
    },

    /**
     * Hàm xóa nhân viên đang được chọn.
     */
    delEmployee() {
      if (this.selectedEmployeeId) {
        axios
          .delete(
            `http://api.manhnv.net/v1/employees/${this.selectedEmployeeId}`
          )
          .then(() => {
            // hiển thị dialog thông báo với câu thông báo.
            this.showAlertDialogWithMsg("Xóa thành công.");

            // fetch lại dữ liệu.
            this.fetchData();
          })
          .catch((err) => {
            // hiển thị thông báo khi thất bại.
            this.showAlertDialogWithMsg("Xóa thất bại.");
            console.log(err);
          });
      }
    },
  },

  computed: {
    startRecord: function () {
      let s = (this.page - 1) * this.limit + 1;
      return s > 1 ? s : 1;
    },
    endRecord: function () {
      let e = this.page * this.limit;
      return e <= this.totalEmployees ? e : this.totalEmployees;
    },
  },

  watch: {
    selectedDepartment: function () {
      this.fetchData();
    },
    selectedPosition: function () {
      this.fetchData();
    },
    "$route.query": function (val) {
      if (val) {
        this.page = parseInt(val.page) || 1;
      } else {
        this.page = 1;
      }
      this.fetchData();
    },
  },
};
</script>

<style scoped>
.content {
  position: absolute;
  left: 226px;
  top: 48px;
  right: 0;
  bottom: 0;
  padding: 16px 16px 0 16px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.content .title-box {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.content .title-box .title {
  font-size: 24px;
  font-family: Google-Sans-Bold;
}

.content .toolbar-box {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
}

.content .toolbar-left,
.content .toolbar-right {
  display: flex;
  align-items: center;
}

.content .data {
  width: 100%;
  overflow: auto;
  flex: 1;
}

.content .footer {
  margin-top: 16px;
  border-top: 5px solid #bbb;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 56px;
}

/* Table */
.table {
  width: 100%;
  border-collapse: collapse;
}

.table thead tr,
.table tbody tr {
  border-bottom: 2px solid #e5e5e5;
  height: 48px;
  text-align: left;
  cursor: pointer;
}

.table tbody tr:hover {
  background-color: #e5e5e5;
}

.table tbody tr:last-child {
  border: none;
}

.table thead tr th,
.table tbody tr td {
  padding-left: 10px;
  padding-right: 8px;
}

.table thead tr th:first-child,
.table tbody tr td:first-child {
  padding-left: 16px;
}

.table thead tr th:last-child,
.table tbody tr td:last-child {
  padding-right: 16px;
}
</style>