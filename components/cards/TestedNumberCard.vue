<template>
  <v-col cols="12" md="6" class="DataCard">
    <time-stacked-bar-chart
      :title="$t('検査実施件数')"
      :title-id="'number-of-tested'"
      :chart-id="'time-stacked-bar-chart-inspections'"
      :chart-data="inspectionsGraph"
      :date="Data.inspections_summary.date"
      :items="inspectionsItems"
      :labels="inspectionsLabels"
      :unit="$t('件.tested')"
      :data-labels="inspectionsDataLabels"
      :table-labels="inspectionsTableLabels"
    >
      <!-- 件.tested = 検査数 -->
      <template v-slot:description>
        <ul :class="$style.GraphDesc">
          <li>
            {{
              $t(
                '（注）検体採取日を基準とする。ただし、一部検査結果判明日に基づくものを含む'
              )
            }}
          </li>
          <li>
            {{ $t('（注）同一の対象者について複数の検体を検査する場合あり') }}
          </li>
          <li>
            {{
              $t(
                '（注）速報値として公開するものであり、後日確定データとして修正される場合あり'
              )
            }}
          </li>
        </ul>
      </template>
    </time-stacked-bar-chart>
  </v-col>
</template>

<script>
import dayjs from 'dayjs'
import duration from 'dayjs/plugin/duration'
import Data from '@/data/data.json'
import TimeStackedBarChart from '@/components/TimeStackedBarChart.vue'
dayjs.extend(duration)

export default {
  components: {
    TimeStackedBarChart
  },
  data() {
    // 検査実施日別状況
    const l = Data.inspections_summary.data['都内'].length
    const domestic = []
    const insurance = []
    for (let i = 0; i < l; i++) {
      domestic.push(
        Data.inspections_summary.data['都内'][i] +
          Data.inspections_summary.data['その他'][i]
      )
      insurance.push(Data.inspections_summary.data['保険適用分'][i])
    }

    const inspectionsGraph = [domestic, insurance]
    const inspectionsItems = [
      this.$t('健康安全研究センターが行った検査件数'),
      this.$t('医療機関等が行った検査件数')
    ]
    const inspectionsLabels = Data.inspections_summary.labels
    const inspectionsDataLabels = [
      this.$t('健康安全研究センターが行った検査件数'),
      this.$t('医療機関等が行った検査件数')
    ]
    const inspectionsTableLabels = [
      this.$t('健康安全研究センター実施分'),
      this.$t('医療機関等実施分')
    ]

    return {
      Data,
      inspectionsGraph,
      inspectionsItems,
      inspectionsLabels,
      inspectionsDataLabels,
      inspectionsTableLabels
    }
  }
}
</script>

<style module lang="scss">
.Graph {
  &Desc {
    margin: 0;
    margin-top: 1rem;
    padding-left: 0 !important;
    color: $gray-3;
    list-style: none;
    @include font-size(12);
  }
}
</style>
