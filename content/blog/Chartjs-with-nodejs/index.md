---
title: Using Chart.js in Nodejs server and exporting it as an image.
date: "2021-01-03T23:46:37.121Z"
description: Working with chart.js on node.js can be hard at first, so here you go with a solution.
---

![demo chart](https://mir-s3-cdn-cf.behance.net/project_modules/disp/82cc7320218015.562e7a26b12a9.jpg)

> There is a very simple way to use chart.js on an HTML page, one just has to include the CDN URL in 'src' and draw on canvas in the script tag. But this script tag runs when you open the file in the browser, what if you want to run the code on the node.js server. For that, you can continue reading my post.

## Installing packages

For making a chart on node.js, first, you will have to install **chart.js** and **chartjs-node-canvas** .

```
npm i chart.js
npm i chartjs-node-canvas
```

Now require it into your file :

```
const {CanvasRenderService} = require('chartjs-node-canvas');
```

For using the chartjs service, you will have to write:

```
const canvasRenderService = new CanvasRenderService(width, height, chartCallback);
```

Here you will be providing width, height, and the callback function.

### Converting chart to image

For converting the chart to an image, you use 'renderToDataURL'. This converts the chart into a base64 image URL. By default, it converts the image to png format. In the configuration, you pass the customized values as per your need.

```
canvasRenderService.renderToDataURL(configuration);
```

The configuration is similar to what you use in html or script. You can find the documentation [here](https://www.chartjs.org/docs/latest/).

### Final Code

```
const { CanvasRenderService } = require('chartjs-node-canvas');
const width = 1000;   // define width and height of canvas
const height = 1000;
const chartCallback = (ChartJS) => {
 console.log('chart built')
};
const canvasRenderService = new CanvasRenderService(width, height, chartCallback);

var xLabels = ['1','2','3','4','5','6','7','8','9','10','11']

const createImage = async () => {
    const configuration = {
        type: 'line',   // for line chart
          data: {
              labels: [150,300,450,600,750,900,1050,1200,1350,1500],
              datasets: [{
                  label: "sample 1",
                  data: [100,43],
                  fill: false,
                  borderColor: ['rgba(255, 99, 132, 1)'],
                  borderWidth: 1,
                  xAxisID: 'xAxis1' //define top or bottm axis ,modifies on scale
              },
              {
                  label: "sample 2",
                  data: [72,83],
                  fill: false,
                  borderColor: ['rgba(265, 99, 132, 1)'],
                  borderWidth: 1,
                  xAxisID: 'xAxis1'
              },
              {
                  label: "sample3",
                  data: [30,56],
                  fill: false,
                  borderColor: ['rgba(235, 99, 122, 1)'],
                  borderWidth: 1,
                  xAxisID: 'xAxis1'
              }
              ],

          },
          options: {
                  scales: {
                  xAxes:[
                      {
                      id:'xAxis1',
                      position: 'bottom',
                      type:"category",

                      },
                      {
                      id:'xAxis2',
                      position: 'top',
                      type:"category",
                      ticks:{
                          callback: function(value, index, values) {
                              return xLabels[index];  // gives points of top x axis
                          }
                  },
              }],
                  yAxes: [{
                  display: true,
                  ticks: {
                      max: 200,
                      stepSize: 10, //defines y axis step scale
                  }
              }]
          ,
              }
          }
          }

    const dataUrl = await canvasRenderService.renderToDataURL(configuration); // converts chart to image
    return dataUrl;
};

module.exports = {
createImage   //for exporting to another file
}
```

And this is how you make it work on node.js server. You can use the image to any file now whether ejs or html. Hope it helps.
