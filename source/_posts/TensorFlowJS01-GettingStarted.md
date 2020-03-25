title: tensorflow.js-01-gettingStarted
author: bellick
tags:
  - tf.js
categories:
  - MachineLearning
  - Tensorflow.js
date: 2018-11-10 19:19:00
mathjax: true
---
$ mkdir tfjs01

$ cd tfjs01

$ npm init -y

$ npm install -g parcel-bundler

$ touch index.html index.js

$ npm install bootstrap

```
//index.js
import * as tf from '@tensorflow/tfjs';
import 'bootstrap/dist/css/bootstrap.css';

// Define a machine learning model for linear regression
const model = tf.sequential();
model.add(tf.layers.dense({units: 1, inputShape: [1]}));

// Specify loss and optimizer for model
model.compile({loss: 'meanSquaredError', optimizer: 'sgd'});

// Prepare training data
const xs = tf.tensor2d([-1, 0, 1, 2, 3, 4], [6, 1]);
const ys = tf.tensor2d([-3, -1, 1, 3, 5, 7], [6,1]);

// Train the model
//model.fit(xs, ys, {epochs: 500}).then(() => {
//});

// Train the model and set predict button to active
model.fit(xs, ys, {epochs: 500}).then(() => {
    // Use model to predict values
    document.getElementById('predictButton').disabled = false;
    document.getElementById('predictButton').innerText = "Predict";
});

// Register click event handler for predict button
document.getElementById('predictButton').addEventListener('click', (el, ev) => {
    let val = document.getElementById('inputValue').value;
    document.getElementById('output').innerText = model.predict(tf.tensor2d([val], [1,1]));
});
```
```
//index.html
<html>
<body>
    <div class="container" style="padding-top: 20px">
        <div class="card">
            <div class="card-header">
                <strong>TensorFlow.js Demo - Linear Regression</strong>
            </div>
            <div class="card-body">
                <label>Input Value:</label> <input type="text" id="inputValue" class="form-control"><br>
                <button type="button" class="btn btn-primary" id="predictButton" disabled>Model is being trained, please wait ...</button><br><br>
                <h4>Result: </span></h4>
                <h5><span class="badge badge-secondary" id="output"></span></h5>
            </div>
        </div>
    </div>

    <script src="./index.js"></script>
</body>
</html>
```

##### run
	parcel index.html