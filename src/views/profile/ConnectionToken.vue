<template>
  <GenericListPage
    ref="GenericListTable"
    :header-actions="headerActions"
    :help-message="helpMessage"
    :table-config="tableConfig"
  />
</template>

<script>
import { GenericListPage } from '@/layout/components'
import { ShowKeyCopyFormatter } from '@/components/Table/TableFormatters'

export default {
  name: 'ConnectionToken',
  components: {
    GenericListPage
  },
  data() {
    const ajaxUrl = '/api/v1/authentication/connection-token/'
    return {
      helpMessage: this.$t('setting.helpText.ConnectionTokenList'),
      tableConfig: {
        url: ajaxUrl,
        columnsExtra: ['action'],
        columnsShow: {
          min: ['id', 'actions'],
          default: [
            'id', 'type_display', 'date_expired', 'is_valid', 'actions'
          ]
        },
        columnsMeta: {
          id: {
            label: 'Token ID',
            formatter: ShowKeyCopyFormatter
          },
          action: {
            label: this.$t('common.Action'),
            formatter: function(row) {
              return row.actions.map(item => {
                return item.label
              }).join(', ')
            }
          },
          actions: {
            formatterArgs: {
              hasUpdate: false,
              hasClone: false,
              hasDelete: false,
              extraActions: [
                {
                  name: 'Expired',
                  title: this.$t('setting.Expire'),
                  type: 'info',
                  can: ({ row }) => !row['is_expired'] && this.$hasPerm('authentication.change_connectiontoken'),
                  callback: function({ row }) {
                    this.$axios.patch(`${ajaxUrl}${row.id}/expire/`,
                    ).then(res => {
                      this.getRefsListTable.reloadTable()
                      this.$message.success(this.$tc('common.updateSuccessMsg'))
                    }).catch(error => {
                      this.$message.error(this.$tc('common.updateErrorMsg' + ' ' + error))
                    })
                  }.bind(this)
                }
              ]
            }
          }
        }
      },
      headerActions: {
        hasLeftActions: false,
        hasSearch: true,
        hasRightActions: true,
        hasRefresh: true,
        hasExport: false,
        hasImport: false,
        hasBulkDelete: false,
        hasCreate: false,
        extraActions: [
        ]
      }
    }
  },
  computed: {
    getRefsListTable() {
      return this.$refs.GenericListTable.$refs.ListTable.$refs.ListTable || {}
    }
  }
}
</script>

<style scoped>

</style>
