# tfjs_model

# 載入模型
```
let model

try{
    model = await tf.loadLayersModel('localstorage://CaptchaModel');
    console.log('Load local model')
}catch(event){
    model = await tf.loadLayersModel(tf.io.browserHTTPRequest('https://raw.githubusercontent.com/syuanyikuo/tfjs_model/main/tixcraft/model.json'));
    console.log('Load github model')
    await model.save('localstorage://CaptchaModel');
}
```

