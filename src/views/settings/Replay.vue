<template>
  <div>
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
          this.$t('setting.replay.Storage'),
          [
            'TYPE', 'HOST', 'PORT',
            'USERNAME', 'PASSWORD', 'PASV', 'DIR'
          ]
        ]
      ],
      successUrl: { name: 'Settings', params: { activeMenu: 'Terminal' }},
      fieldsMeta: {
        TYPE: {
          label: this.$t('setting.replay.Type'),
          type: 'select',
          options: [
            { label: this.$t('setting.replay.Local'), value: 'local' },
            { label: 'FTP', value: 'ftp' }
          ]
        },
        HOST: {
          label: this.$t('setting.replay.Host'),
          type: 'input',
          rules: [Required],
          hidden: form => {
            return form['TYPE'] !== 'ftp'
          },
          helpText: this.$t('setting.helpText.FTPAddr')
        },
        PORT: {
          label: this.$t('setting.replay.Port'),
          type: 'input',
          hidden: form => {
            return form['TYPE'] !== 'ftp'
          }
        },
        USERNAME: {
          label: this.$t('setting.replay.Username'),
          type: 'input',
          hidden: form => {
            return form['TYPE'] !== 'ftp'
          }
        },
        PASSWORD: {
          label: this.$t('setting.replay.Password'),
          type: 'input',
          hidden: form => {
            return form['TYPE'] !== 'ftp'
          }
        },
        PASV: {
          label: this.$t('setting.replay.Pasv'),
          type: 'checkbox',
          hidden: form => {
            return form['TYPE'] !== 'ftp'
          }
        },
        DIR: {
          label: this.$t('setting.replay.Dir'),
          type: 'input',
          hidden: form => {
            return form['TYPE'] !== 'ftp'
          }
        }
      },
      url: '/api/v1/settings/setting/'
    }
  },
  methods: {
    cleanFormValue(data) {
      return {
        replay: { SERVER_REPLAY_STORAGE: data }
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
