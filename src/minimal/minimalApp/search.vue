<template>
  <div>
    <div v-show="loading" id="loadMalSearchPop" class="mdl-progress mdl-js-progress mdl-progress__indeterminate" style="width: 100%; position: absolute;"></div>
    <slot></slot>
    <div class="mdl-grid">
      <span v-if="!loading && !items.length" class="mdl-chip" style="margin: auto; margin-top: 16px; display: table;"><span class="mdl-chip__text">{{lang("NoEntries")}}</span></span>

      <a v-for="item in items" :key="item.id" class="mdl-cell bg-cell mdl-cell--6-col mdl-cell--8-col-tablet mdl-shadow--2dp mdl-grid searchItem" :href="item.url" :data-mal="item.malUrl" style="cursor: pointer;">
        <img :src="item.image" style="margin: -8px 0px -8px -8px; height: 100px; width: 64px; background-color: grey;"></img>
        <div style="flex-grow: 100; cursor: pointer; margin-top: 0; margin-bottom: 0;" class="mdl-cell">
          <span style="font-size: 20px; font-weight: 400; line-height: 1;">{{item.name}}</span>
          <p style="margin-bottom: 0; line-height: 20px; padding-top: 3px;">{{lang("search_Type")}} {{item.media_type}}</p>
          <p style="margin-bottom: 0; line-height: 20px;">{{lang("search_Score")}} {{item.score}}</p>
          <p style="margin-bottom: 0; line-height: 20px;">{{lang("search_Year")}} {{item.year}}</p>
        </div>
      </a>

    </div>
  </div>
</template>

<script type="text/javascript">
  import {search}  from "./../../provider/provider";
  export default {
    components: {
    },
    data: function(){
      return {
        items: [],
        loading: true,
      }
    },
    props: {
      type: {
        type: String,
        default: 'anime'
      },
      keyword: {
        type: String,
        default: ''
      },
    },
    mounted: function(){
      this.load();
    },
    activated: function(){
      this.$nextTick(() => {
        j.$(this.$el).closest('html').find("head").click();
      })
    },
    watch: {
      keyword: function(type){
        this.load();
      },
      type: function(type){
        this.load();
      }
    },
    methods: {
      lang: api.storage.lang,
      load: function(){
        this.loading = true;

        search(this.keyword, this.type).then((items) => {
          this.loading = false;
          this.items = items;
        })
      }
    }
  }
</script>
