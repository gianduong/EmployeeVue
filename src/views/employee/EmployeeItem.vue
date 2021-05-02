<template>
  <tr :class="{'selected': selectedId == employee.EmployeeId}" @dblclick="$emit('dblclick', employee.EmployeeId)" @click="$emit('click', employee.EmployeeId)">
    <td>{{ employee.EmployeeCode }}</td>
    <td>{{ employee.FullName }}</td>
    <td>{{ genderName }}</td>
    <td>{{ employee.DateOfBirth | formatDateOfBirth }}</td>
    <td>{{ employee.PhoneNumber }}</td>
    <td>{{ email }}</td>
    <td>{{ positionName }}</td>
    <td>{{ departmentName }}</td>
    <td class="has-text-right">{{ employee.Salary | formatSalary }}</td>
    <td>{{ employee.WorkStatus }}</td>
  </tr>
</template>

<script>
export default {
  name: "EmployeeItem",
  props: {
    /**
     * Thông tin nhân viên.
     */
    employee: Object,

    /**
     * Id của nhân viên đang được chọn.
     */
    selectedId: {
      type: String,
      default: null
    }
  },
  computed: {
    /**
     * Computed binđ giới tính. Mặc định là Không xác định.
     */
    genderName: function(){
      return this.employee.GenderName || "Không xác định";
    },

    /**
     * Computed bind email nhân viên. Mặc định là không rõ.
     */
    email: function(){
      return this.employee.Email || "Không rõ";
    },

    /**
     * Computed bind vị trí của nhân viên. Mặc định là không rõ.
     */
    positionName: function(){
      return this.employee.PositionName || "Không rõ";
    },

    /**
     * Computed bind phòng ban của nhân viên. Mặc định là không rõ.
     */
    departmentName: function(){
      return this.employee.DepartmentName || "Không rõ";
    }
  },
  filters: {
    /**
     * Filter giúp chuyển date String về dạng DD-MM-YYYY. Mặc định là null.
     */
    formatDateOfBirth(dateOfBirth) {
      if (dateOfBirth) {
        let date = new Date(dateOfBirth);
        let dateString =
          date.getDate() < 10
            ? "0" + date.getDate().toString()
            : date.getDate().toString();
        let monthString =
          date.getMonth() < 9
            ? "0" + (date.getMonth() + 1).toString()
            : (date.getMonth() + 1).toString();
        let yearString = date.getFullYear();
        return `${dateString}/${monthString}/${yearString}`;
      }
      return "Không xác định";
    },

    /**
     * Filter chuyển định dạng tiền tệ. xxx.xxx.xxx
     */
    formatSalary(salary){
      if(salary){
        return new Intl.NumberFormat('vi-VN').format(salary);
      }
      return "Không rõ";
    }
  },
};
</script>

<style scoped>
tr td {
  padding-left: 10px;
  padding-right: 8px;
}

tr td:first-child {
  padding-left: 16px;
}

tr td:last-child {
  padding-right: 16px;
}
</style>