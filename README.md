# tfjs_model
售票系統驗證碼辨識，可在瀏覽器上直接運行

# Requirements
- TensorFlow.js　https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@3.12.0/dist/tf.min.js
- html2canvas　https://html2canvas.hertzen.com/dist/html2canvas.js
- OpenCV.js　https://docs.opencv.org/3.4.0/opencv.js

# Load model
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

