<template>
    <div>
        <canvas ref="myChart" class="m-4" />
    </div>
  </template>
  <script>
  import Chart from 'chart.js/auto'
  import _ from 'lodash'
  export default {
    
    props: ['data'],
    mounted () {
        const tanggal = [];
        const jumlah_positif = [];
        for(const data of this.data){
          tanggal.push(data.tanggal);
          jumlah_positif.push(data.jumlah_positif.value);
        }
        const sort = jumlah_positif.sort((a,b)=> a - b);
        let q1 = ((sort.length)-1)*0.25;
        let q3 = ((sort.length)-1)*0.75;
        let baseq1 = Math.floor(q1);
        let baseq3 = Math.floor(q3);
        let restq1 = q1 - baseq1;
        let restq3 = q3 - baseq3;
        let lowerBound = 100, upperBound = 300;
        if((sort[baseq1+1]!==undefined)){
          lowerBound =  sort[baseq1] + restq1 * (sort[baseq1+1] - sort[baseq1]);
        } else {
          lowerBound = sort[baseq1]
        }
        
        if((sort[baseq3+1]!==undefined)){
          upperBound =  sort[baseq3] + restq3 * (sort[baseq3+1] - sort[baseq3]);
        } else {
          upperBound = sort[baseq3]
        } 
        
        console.log(sort);
        console.log(upperBound);

        const normalY = (x, mean, stdDev) => Math.exp((-0.5) * Math.pow((x - mean) / stdDev, 2)) * 1;

        const getMean = (lowerBound, upperBound) => (upperBound + lowerBound) / 2;

        // distance between mean and each bound of a 95% confidence interval 
        // is 2 stdDeviation, so distance between the bounds is 4
        const getStdDeviation = (lowerBound, upperBound) => (upperBound - lowerBound) / 4;



        const generatePoints = (lowerBound, upperBound) => {
        let stdDev = getStdDeviation(lowerBound, upperBound); 
        let min = lowerBound - 2 * stdDev;
        let max = upperBound + 2 * stdDev;
        let unit = (max - min) / 100;
        return _.range(min, max, unit);
        }

        let mean = getMean(lowerBound, upperBound);
        let stdDev = getStdDeviation(lowerBound, upperBound);
        let points = generatePoints(lowerBound, upperBound);


        let seriesData = points.map(x => ({ x, y: normalY(x, mean, stdDev)}));
        const myChart = this.$refs.myChart
      
        const data = {
        
        datasets: [{
            label: 'Distribusi normal',
            data: seriesData,
            fill: true,
            borderColor: 'rgb(75, 192, 192)',
            tension: 0.1,
            showLine : true
        }]
        };

      /* eslint-disable no-unused-vars */
      const radarChart = new Chart(myChart, {
        type: 'scatter',
        data
      })
      /* eslint-disable no-unused-vars */
      
    }
  }
  </script>
  