# tfjs_model
售票系統驗證碼辨識，可在瀏覽器上運行

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
# Use
```
var ENG = 'abcdefghijklmnopqrstuvwxyz'

var canvas = document.createElement("canvas")
canvas.width=80
canvas.height=50;
var ctx = canvas.getContext("2d");
var image = new Image();
image.onload = function() {
    ctx.drawImage(image, 0, 0,80,50);

    let src = cv.imread(canvas);
    let gray = new cv.Mat();
    let thresh = new cv.Mat();

    cv.cvtColor(src, gray, cv.COLOR_RGBA2GRAY);
    cv.threshold(gray, thresh, 230, 255, cv.THRESH_BINARY_INV);

    for (let i = 0; i < thresh.cols; i++) {
        thresh.ucharPtr(0, i)[0] = 255;
    }

    let height = thresh.rows;
    let width = thresh.cols;
    let threshArray = new Array(height);

    for (let i = 0; i < height; i++) {
        threshArray[i] = new Array(width);
        for (let j = 0; j < width; j++) {
            threshArray[i][j] = thresh.data[i * width + j];
        }
    }

    prediction = model.predict(tf.tensor([threshArray],[1,50,80,1],'float32'))

    var captcha = ''

    prediction.forEach(pred => {
        var data = pred.dataSync();
        var maxIndex = data.indexOf(Math.max(...data))
        captcha += ENG[maxIndex];
    });

   document.querySelector("#TicketForm_verifyCode").value = captcha

};
image.src = reader.result
```
