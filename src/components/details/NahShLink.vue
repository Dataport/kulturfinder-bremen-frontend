<template>
  <info-detail img-alt="NAH.SH">
    <template #icon>
      <icon-nah-sh/>
    </template>
    <template #text>
      <a :href="nahShLink" target="_blank">{{ linkText }}</a>
    </template>
  </info-detail>
</template>

<script>
import moment from 'moment'
import i18n from '@/i18n'
import InfoDetail from '@/components/details/InfoDetail.vue'

export default {
  name: 'NahShLink',
  props: {
    linkText: {
      type: String,
      default: i18n.t('nahSh.showDepartures')
    },
    startPos: {
      type: [Object, Boolean],
      default: false
    },
    endPos: {
      type: Object,
      default: () => ({})
    },
    endName: {
      type: String,
      default: ''
    }
  },
  computed: {
    nahShLink() {
      return `https://nah.sh.hafas.de/bin/query.exe/dn?SID=${this.nahShSID}&ZID=${this.nahShZID}&date=${this.nahShDate}&time=${this.nahShTime}&timesel=depart&externalCall=yes&start=yes`
    },
    nahShSID() {
      if (!this.startPos) return ''

      const lat = this.formatNahShCoordinate(this.startPos.lat)
      const lng = this.formatNahShCoordinate(this.startPos.lng)
      const start = this.$t('nahSh.location.start')

      return `A=16@O=${start}@X=${lng}@Y=${lat}@`
    },
    nahShZID() {
      if (!this.endPos) return ''

      const lat = this.formatNahShCoordinate(this.endPos.lat)
      const lng = this.formatNahShCoordinate(this.endPos.lng)

      return `A=16@O=${this.endName}@X=${lng}@Y=${lat}@`
    },
    nahShDate() {
      return moment().format('DD-MM-YYYY')
    },
    nahShTime() {
      return moment().format('HH:mm')
    }
  },
  methods: {
    /*
     * The NAH-SH api expects 6 decimal places and no decimal separator
     */
    formatNahShCoordinate(value) {
      return (Math.round(value * 1000000) / 1000000).toFixed(6)
        .toString()
        .replace('.', '')
    }
  },
  components: {
    InfoDetail
  }
}
</script>