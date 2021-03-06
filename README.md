# 稿定编辑器 SDK 接入说明
稿定编辑器 SDK 是对稿定能力的封装，以实现对设计服务的无缝对接，目前支持以下服务：

- [x] 平面编辑器
- [x] 图片编辑器
- [ ] TODO: H5编辑器
- [ ] TODO: 打通用户登录, 请联系定制

NPM 包参见：[@gaoding/editor-sdk](https://www.npmjs.com/package/@gaoding/editor-sdk)

### 平面编辑器
1. 海量创意模板快速生成
1. 支持指定数十种主流模板场景类目 (https://gaoding.com/templates)


### 图片编辑器
- [x] 1. 图片剪裁功能
- [x] 2. 图片增强、图片滤镜效果
- [x] 3. 图片添加水印、添加相框、添加文字
- [x] 4. 海量创意素材（文字、贴纸、边框、水印...）
- [x] 5. 马赛克、涂鸦
- [x] 6. 批量编辑

##

### 在接入前，要做什么？
请先与我们联系（邮箱：`bd@gaoding.com`），
**我们需要合作方提供企业以及接入域名等信息**，
以便我们安排专人接待，为合作方预设一些关键数据，如： `APPID`、`模板类目ID`

### 对接与调试
如合作方已经依本文档的定义实现了接口，我们需要合作方提供接入的域名信息，并记录到稿定的系统中，然后，由专人负责与合作方对接调试

### 安装
```shell
npm i @gaoding/editor-sdk
# or
yarn add @gaoding/editor-sdk
```

### cdn 使用方式
```html
<script src="./jquery.min.js"></script>
<script src="https://cdn.dancf.com/editor-sdk@0.2.5/dist/gd-editor-sdk.min.js"></script>
<script>
    var gdEditor = new GdEditorSdk({
        // 区分编辑器类型 (图片编辑器、平面编辑器、H5编辑器)
        appId: '由SDK方提供',
        // onCompleted 执行后，是否自动关闭弹窗，默认为 true
        autoClose: true,
        // 完成按钮文案默认“完成”
        buttonText: '完成',

        // 0.2.4 版本以上支持
        onCompleted: function (params) {
            // 上传的例子
            const form = new FormData();
            form.append('file', params.blob, 'gaoding.jpg');

            $.ajax({
                url: 'url',
                type: 'post',
                contentType: 'multipart/form-data',
                data: form,
                success: function (res) {
                    alert("ok")
                }
            });
        },

        // 自定义iFrame样式
        style: {}
    });
    gdEditor.open({
        ext: {
            third_cate_id: 356
        }
    });
</script>
```
### npm 使用方式
```javascript
import { GdEditorSdk } from '@gaoding/editor-sdk';

const gdEditorSDK = new GdEditorSdk({
    // 区分编辑器类型 (图片编辑器、平面编辑器、H5编辑器)
    appId: '由SDK方提供',
    // onCompleted 执行后，是否自动关闭弹窗，默认为 true
    autoClose: true,
    // 完成按钮文案默认“完成”
    buttonText: '完成',
    
    // 完成编辑器下载时触发
    onCompleted({ blob, workId, sourceId }) {
        // 直接展示
        const url = window.URL.createObjectURL(blob);
        const img = document.createElement('img');
        img.src = url;
        document.body.append(img);

        // 上传的例子
        const form = new FormData();
        form.append('file', blob, 'gaoding.jpg');
        axios.post('url', form, { headers: { 'Content-Type': 'multipart/form-data' } })
            .then(res => {
                alert('上传成功');
            })
            .catch(error => {
                alert(error.message);
            })
    },
    // 自定义iFrame样式
    style: {}
});

// 打开弹窗
gdEditorSDK.open({
    ext: {
        // 指定海报模板类目(图片、简历、GIF、LOGO、海报、表情头像、文章配图、适配封面。。。)
        // ps: 使用场景为平面编辑器, 分类 ID 由 SDK方提供
        // third_cate_id: '',
        // 指定编辑器底图
        // ps: 使用场景为图片编辑器，需开启 CORS 跨域访问，稿定域名为 *.gaoding.com
        url: 'https://st-gdx.dancf.com/materials/115030/shots/20190830-155521-WWU47.png'
    }
});
// 关闭弹窗
gdEditorSDK.close();
```
