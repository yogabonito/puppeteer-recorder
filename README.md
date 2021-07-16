# puppeteer-recorder
Record frame-by-frame animations using puppeteer. Based on electron-recorder.

# Usage
```javascript
const puppeteer = require('puppeteer');
const { record } = require('puppeteer-recorder');

async function prepare_and_record(url, video_width=3840, video_height=2160) {
  console.log("##########################################################");
  console.log("# START");
  
  const fps = 30;
  const n_seconds = 10;
  let browser = await puppeteer.launch();
  
  let page = await browser.newPage();
  await page.setViewport({width: video_width, height: video_height});
  await page.goto(url);

  await record({
    browser: browser, // Optional: a puppeteer Browser instance,
    page: page, // Optional: a puppeteer Page instance,
    output: 'output.mp4',
    fps: fps,
    frames: fps * n_seconds,
    logEachFrame: true,
    prepare: function (browser, page) { /* executed before first capture */ },
    render: function (browser, page, frame) { /* executed before each capture */ }
  });
}

await prepare_and_record("http://localhost:3000",
                         2560,//1920,
                         1440);//1080);

console.log("##########################################################");
console.log("# DONE.");
```
