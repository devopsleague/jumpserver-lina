<template>
  <div>
    <el-alert :title="pageInfo" type="info" show-icon />
    <IBox>
      <GenericCreateUpdateForm
        :fields="selectFields"
        :url="url"
        :initial="object"
        :update-success-next-route="successUrl"
        :clean-form-value="cleanFormValue"
        :object="object"
        :fields-meta="fieldsMeta"
        :get-method="getMethod"
      />
    </IBox>
  </div>
</template>
<script>
import GenericCreateUpdateForm from '@/layout/components/GenericCreateUpdateForm'
import { IBox } from '@/components'
import { Required } from '@/components/DataForm/rules'
export default {
  name: 'Logging',
  components: {
    GenericCreateUpdateForm,
    IBox
  },
  props: {
    object: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      selectFields: [
        [
          this.$t('setting.logging.Syslog'),
          [
            'SYSLOG_ENABLE', 'SYSLOG_ADDR', 'SYSLOG_SOCKTYPE', 'SYSLOG_FACILITY'
          ]
        ]
      ],
      successUrl: { name: 'Settings', params: { activeMenu: 'Terminal' }},
      fieldsMeta: {
        SYSLOG_ENABLE: {
          label: this.$t('setting.logging.SyslogEnable'),
          type: 'checkbox'
        },
        SYSLOG_ADDR: {
          label: this.$t('setting.logging.SyslogAddr'),
          type: 'input',
          rules: [Required],
          hidden: form => {
            return !form['SYSLOG_ENABLE']
          },
          helpText: this.$t('setting.helpText.SyslogAddr')
        },
        SYSLOG_FACILITY: {
          label: this.$t('setting.logging.SyslogFacility'),
          type: 'input',
          hidden: form => {
            return !form['SYSLOG_ENABLE']
          }
        },
        SYSLOG_SOCKTYPE: {
          label: this.$t('setting.logging.SyslogSockType'),
          type: 'select',
          options: [
            { label: 'UDP', value: 2 },
            { label: 'TCP', value: 1 }
          ],
          hidden: form => {
            return !form['SYSLOG_ENABLE']
          }
        }
      },
      url: '/api/v1/settings/setting/',
      pageInfo: this.$t('setting.helpText.RestartInfo')
    }
  },
  methods: {
    cleanFormValue(data) {
      return {
        logging: data
      }
    },
    getMethod() {
      return 'put'
    }
  }
}
</script>

<style scoped>

</style>
