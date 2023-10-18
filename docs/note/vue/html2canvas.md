# html2canvas

!> 解决html2canvas图片跨域问题

`html2canvas`在截图的过程中，如果遇到html中有跨域地址的图片，即图片地址非本地，截图的时候将不会显示图片

## 解决方法

### html

````html
<div ref="imageDom">
  <!-- 支持跨域的地址 -->
  <img
      :src="imgUrl+'?'+new Date().getTime()"
      crossOrigin="anonymous"   
  />
  <label> Hello world ! </label>
</div>
````

### js

````js
import html2canvas from 'html2canvas';
export default {
  data () {
    return {
      imgUrl:'×××××××.png' // 跨域地址
    }
  },
  methods: {
    async html2picture () {
      // 将html转化为canvas
      const canvas = await html2canvas(this.$refs.imageDom, {
        useCORS:true // 支持跨域
      })
      //将canvas转化为base64的jpg（可自行设置格式）
      return canvas.toDataURL('image/jpg');
    }        
  }
}
````

## ps：

1. `allowTaint:true` 和 `useCORS:true` 都是解决跨域问题的方式，不同的是使用allowTaint 会对canvas造成污染，导致无法使用canvas.toDataURL 方法，所以这里不能使用allowTaint: true
2. 在跨域的图片里设置 `crossOrigin="anonymous"` 并且需要给imgUrl加上随机数
3. canvas.toDataURL('image/jpg') 是将canvas转成base64图片格式