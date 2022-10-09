<template>
  <div class="container-sm xl:container-xl bg-blue-900 p-10">
    <div class="radar-graph p-5 bg-white rounded">
      <h1 class="text-center text-xl">Data Kasus Covid-19 per Provinsi</h1>
      <div class="flex flex-wrap justify-center">
        <div v-for="data in datas" :key="data.name" class="w-full h-full md:w-1/4 md:h-1/4  rounded-lg shadow-lg bg-blue-300 m-1 sm:m-5"> 
            <RadarGraph :kelompok_umur="data.data_umur" :nama="data.name"  />
        </div>
      </div>
    </div>
    <div class="radar-graph p-5 bg-white rounded">
      <h1 class="text-center text-xl">Distribusi Normal</h1>
      <NormalDistribution  :data ="datas_dist"/>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import RadarGraph from '../components/RadarGraph.vue'
import NormalDistribution from '../components/NormalDistribution.vue'
export default {
 
    
    async asyncData() {
      const kelompok_umur = [];
      const nama = []
      const list_data = []
      let result = []
      let result_data
        const  data_dua  = await axios.get("https://data.covid19.go.id/public/api/update.json");
        const { data } = await axios.get("https://data.covid19.go.id/public/api/prov.json");
        const data_harian = data_dua.data.update.harian;
        const data_distribusi = [];
        for(const data of data_harian){
          data_distribusi.push({tanggal : data.key_as_string, jumlah_positif : data.jumlah_positif});
        }
        for (const kelompok of data.list_data) {
         kelompok_umur.push(kelompok.kelompok_umur)
         nama.push(kelompok.key)
        }
        for(const doc_count of kelompok_umur){
          let objArray = doc_count.map(a => a.doc_count)
          result.push(objArray)
        }
        
        for(const index in result){
          list_data.push(
            {
              name : nama[index],
              data_umur : result[index]
            }
          )
        }
        
        return { datas: list_data, datas_dist : data_distribusi };
    },
    
    components: { RadarGraph, NormalDistribution }
}
</script>
