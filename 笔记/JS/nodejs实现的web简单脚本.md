使用了puppeteer库（https://github.com/GoogleChrome/puppeteer），

**1.首先调用api创建谷歌浏览器，然后创建一个页面**

```javascript
const browser = await puppeteer.launch({
    args: ["--proxy-server='direct://'", '--proxy-bypass-list=*']
    // headless: false //开启可视化页面
});
const page = await browser.newPage();
```

**2.通过选择器找到搜索框，搜索按钮页面元素，输入内容**

```javascript
const searchInputSel = '#searchText'; //选择器，通过f12后选中元素，copy selector获得
let inputEle = await page.$(searchInputSel); //找到元素对象
await inputEle.type('第五人格');//输入
```

**3.然后点击，点击会出现不同结果：**

这类搜索按钮点击后通常有3个结果：1）本tab转到一个新地址，并载入搜索结果；2）弹出一个新tab，里面是搜索结果；3）本tab的hash变化（即#符号后面的内容变化），并载入搜索结果

这3种结果的处理方式不尽相同，这里分别举例：

1）转到新地址

直接贴示例代码：

```javascript
const [response] = await Promise.all([
    page.waitForNavigation({timeout:0, waitUntil:'networkidle0'}),
    page.click(searchEnterSel),
]);
```

这是官方文档的建议写法，用文字解释的话大致是：等到点击以及页面跳转全部完成

2）弹出新tab

这里的示例代码源于StackOverflow上的一个回答，如下：

```javascript
const homeTarget = page.target();
await page.click(searchEnterSel);
const newTarget = await browser.waitForTarget(target => target.opener() == homeTarget);
const searchResultPage = await newTarget.page();
await searchResultPage.reload({timeout:0, waitUntil:'networkidle0'});
```

其中，最后一个reload可以换成一些waitFor之类的操作，总之就是等到页面中有我们想要的元素。

这个过程用文字解释的话大致是：点击后等到开启者为本页面的弹出页面载入完成

3）本tab的hash变化

需要明确的一点是，hash变化并不是navigation，因此不能用waitForNavigation这个函数，需要改用如下方法：

```javascript
const [response] = await Promise.all([
    page.waitForSelector(wantedSelector),
    page.click(searchEnterSel),
]);
```

重点在于找到wantedSelector，这个selector的要求是：它出现即代表搜索过程结束（或者大致结束，只需等待一些贴图的载入）

**4.找到需要的信息**

目前碰到两种情况：1）文字被诸如<span>的标签包起来；2）文字为标签的一个属性值

针对这两种情况，有着不同的解决方案，如下：

1）文字被标签包起来

这里直接贴示例代码

```javascript
const nameSel = '...';
let nameEle = await page.$(nameSel);
if (nameEle) {
    let nameText = await (await nameEle.getProperty('innerText')).jsonValue();
}
```

这里需要注意的是两种await

2）文字作为标签的一个属性

比如，vivo应用商店的搜索结果中，应用名就作为标签的data-name属性存在，这里示例代码如下：

```javascript
const nameSel = '...';
let nameText = await page.evaluate(`document.querySelector("${nameSel}").getAttribute("data-name")`);
```

可以在想要看看的位置生成当前页面的截图。生成当前页面的截图的方法如下：

```javascript
await page.screenshot({path: 'shot.png'});
```

