<template>
    <div>
        <canvas ref="myChart" class="m-4" />
    </div>
  </template>
  <script>
  import Chart from 'chart.js/auto'
  
  export default {
    // eslint-disable-next-line vue/require-prop-types
    props: ['kelompok_umur', 'nama'],
    mounted () {
      const myChart = this.$refs.myChart
      const data = {
        labels: ['0-5 Tahun', '6-18 Tahun', '19-30 Tahun', '31-45 Tahun', '46-59 Tahun', '>60 Tahun'],
        datasets: [{
          label: `Jumlah kasus Covid ${this.nama}`,
          data: this.kelompok_umur,
          backgroundColor: [
            'rgba(255, 99, 132, 0.2)',
            'rgba(54, 162, 235, 0.2)',
            'rgba(255, 206, 86, 0.2)',
            'rgba(75, 192, 192, 0.2)',
            'rgba(153, 102, 255, 0.2)',
            'rgba(255, 159, 64, 0.2)'
          ],
          borderColor: [
            'rgba(255, 99, 132, 1)',
            'rgba(54, 162, 235, 1)',
            'rgba(255, 206, 86, 1)',
            'rgba(75, 192, 192, 1)',
            'rgba(153, 102, 255, 1)',
            'rgba(255, 159, 64, 1)'
          ],
          borderWidth: 1
        }]
      }
      /* eslint-disable no-unused-vars */
      const radarChart = new Chart(myChart, {
        type: 'radar',
        data
      })
      /* eslint-disable no-unused-vars */
    }
  }
  </script>
  