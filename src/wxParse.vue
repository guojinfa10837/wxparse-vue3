<!--**
 * author: F-loat <chaimaoyuan@foxmail.com>
 *
 * github地址: https://github.com/F-loat/mpvue-wxParse
 *
 * for: Mpvue框架下 微信小程序富文本解析
 */-->

<template>
<!--基础元素-->
<view class="wxParse" :class="className" >
  <view v-for="node of nodes" :key="node.index">
    <wx-parse-template :node="node"/>
  </view>
</view>
</template>

<script>
import {toRefs,ref} from 'vue'
import HtmlToJson from './libs/html2json';
import wxParseTemplate from './components/wxParseTemplate';

export default {
  name: 'wxParse',
  props: {
    loading: {
      type: Boolean,
      default: false,
    },
    className: {
      type: String,
      default: '',
    },
    content: {
      type: String,
      default: '',
    },
    noData: {
      type: String,
      default: '<div style="color: red;">数据不能为空</div>',
    },
    startHandler: {
      type: Function,
      default() {
        return (node) => {
          node.attr.class = null;
          node.attr.style = null;
        };
      },
    },
    endHandler: {
      type: Function,
      default: null,
    },
    charsHandler: {
      type: Function,
      default: null,
    },
    imageProp: {
      type: Object,
      default() {
        return {
          mode: 'aspectFit',
          padding: 0,
          lazyLoad: false,
          domain: '',
        };
      },
    },
  },
  components: {
    wxParseTemplate,
  },
  setup(props,{emit}){
    const imageUrls = ref([])
    const navigate = (node)=>{
      emit('navigate', node);
    };
    const preview = (src, $event) =>{
      if (!imageUrls.value.length) return;
      wx.previewImage({
        current: src,
        urls: imageUrls.value,
      });
      emit('preview', src, $event);
    }
    const removeImageUrl = (src) =>{
      let imgurls= imageUrls.value 
      imgurls.splice(imgurls.indexOf(src), 1);
    }
    return {navigate,imageUrls,preview,removeImageUrl}
  },
  computed: {
    nodes() {
      const {
        content,
        noData,
        imageProp,
        startHandler,
        endHandler,
        charsHandler,
      } = this;
      const parseData = content || noData;
      const customHandler = {
        start: startHandler,
        end: endHandler,
        chars: charsHandler,
      };
     
     const results = HtmlToJson(parseData, customHandler, imageProp, this);
     this.imageUrls = results.imageUrls;
     console.log(results.nodes)
     return results.nodes;
    },
  }
};
</script>
