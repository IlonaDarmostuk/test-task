<template>
  <div>
    <div>{{topAlbum.name}}</div>
    <Series :data=newSeries />

    <Teaser v-for="(item, key) in [teasers[0], teasers[1] ]" :key="key" :data="item" />

    <Series :data=notGender />

    <Teaser v-if="notGender.series.length" :data="teasers[2]"  />

    <Series v-for="item  in restNotGender" :key="item.id" :data=item />
    <Teaser v-if="!notGender.series.length && restNotGender.length" :data="teasers[2]"  />

    <Series   v-for="item in restGender" :key="item.id" :data=item />
    <Teaser v-if="!notGender.series.length && !restNotGender.length && restGender.length" :data="teasers[2]"  />


  </div>
</template>

<script>
import Series from '../components/Series';
import Teaser from '../components/Teaser'
export default {
  components: {
    Series,
    Teaser
  },
  data(){
    return {
      topAlbum: {},
      newSeries: {},
      teasers: {},
      notGender: {},
      restGender: [],
      restNotGender: []
    }
  },
  async fetch(){
    await this.getData();
    await this.getData(2);
  },
  methods: {
    async getData(initialValue = 0){

      try {
        if (!initialValue) {
          await this.getTopAlbum();
          await this.getNewSeries();
        } else {
          await this.getTeasers();
          await this.getNotGender();
          await this.restSeries();
        }
      } catch (e) {
        console.log(e)
      }

    },
    async getTopAlbum(){
       const topAlbum = (await this.$axios.get('/api/catalog/new/bd')).data.topAlbum;
       this.topAlbum = topAlbum;
    },
    async getNewSeries(){
      const newSeries = (await this.$axios.get('/api/catalog/next/bd/0')).data;
      this.newSeries = newSeries;
    },
    async getTeasers(){
      const teasers = (await this.$axios.get('/api/catalog/marketing/bd')).data.list;
      this.teasers = teasers;
    },
    async getNotGender(){
      const notGender = (await this.$axios.get('/api/catalog/next/bd/1')).data;
      this.notGender = notGender;
    },
    async restSeries(initialIndex = 2){

      const restSeries = (await this.$axios.get('/api/catalog/next/bd/'+initialIndex)).data;
      if( Object.keys(restSeries).length){

        if(restSeries.type === 'gender'){
          this.restGender.push(restSeries)
        } else {
          this.restNotGender.push(restSeries)
        }

        this.restSeries(initialIndex+1);
      }

    }
  }
}
</script>
