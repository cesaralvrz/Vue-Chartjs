<script>
import { Bar } from 'vue-chartjs'

export default {
    extends: Bar,
    data: () => ({
    chartdata: {
      labels: ['January', 'February'],
      datasets: [
        {
          label: 'Data One',
          backgroundColor: '#f87979',
          data: [39, 39]
        },
        {
          label: 'Data two',
          backgroundColor: '#f85969',
          data: [39, 39]
        },
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false
    }
  }),
  mounted () {
    this.renderChart(this.chartdata, this.options)
  }
}
</script>