<template>
    <!--其他标签-->
   
      <view :class="node.classStr" :style="node.styleStr">
        <block v-for="(node, index) of node.nodes" :key="index">
          <wx-parse-template :node="node" />
        </block>
      </view>
</template>

<script>
import wxParseTemplate from './wxParseTemplate';
import wxParseImg from './wxParseImg';
import wxParseVideo from './wxParseVideo';
import wxParseAudio from './wxParseAudio';

export default {
  name: 'wxParseTemplate',
  props: {
    node: {},
  },
  components: {
    wxParseTemplate,
    wxParseImg,
    wxParseVideo,
    wxParseAudio,
  },
  methods: {
    wxParseATap(e) {
      const { href } = e.target.dataset;
      if (!href) return;
      console.log(href)
      this.node.$host.navigate(href, e);
    },
  },
};
</script>
