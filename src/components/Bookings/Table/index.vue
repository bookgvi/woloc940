<template lang="pug">
  .bookings
    data-table(
      title="Бронирования"
      :loadData="$app.bookings.getAll"
      :filter="$app.filters.getValues('bookings')"
      :columns="columns"
      :details="details"
      @toggleDialogRow="toggleDialogRow"
    )
      template(#row-dialog="props")
        bookings-dialog(v-bind="props")
      template(#row-controls="props")
        q-btn(flat round icon="comment" title="Открыть чат")
        q-btn(flat round icon="delete" title="Удалить")
        q-btn(flat round icon="edit" @click="props.toggleDialogRow(props.row.id)" title="Редактировать")
        q-btn(flat round icon="thumb_up")
    q-dialog(v-model="isModal")
      q-card(style="min-width: 680px;")
        BookingsDialog(
          :row="bookingRowData"
        )
</template>

<script>
import columns from './columns.js'
import details from './details'
import DataTable from 'components/DataTable'
import BookingsDialog from './BookingsDialog'

export default {
  name: 'bookings-table',
  components: { DataTable, BookingsDialog },
  data: () => ({
    columns,
    details,
    isModal: false,
    bookingRowData: {},
    room: {}
  }),
  methods: {
    toggleDialogRow (row) {
      this.bookingRowData = row
      this.$nextTick(_ => {
        this.isModal = true
      })
    }
  },
  created () {
    if (this.$route.query.customer) {
      this.$app.filters.filterCustomer(this.$route.query.customer)
    } else {
      this.$app.filters.filterDefault('bookings')
    }
  }
}
</script>
