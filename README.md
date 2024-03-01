# How To Solve CAPTCHA while Scraping Amazon?  
  
When conducting searches on Amazon, users often encounter CAPTCHAs that prompt them to enter certain characters or solve puzzles. These measures are in place to distinguish between human users and automated bots. CAPTCHAs are designed to present challenges that are easy for humans to solve but difficult for computers. By successfully completing a CAPTCHA, users verify their human identity, ensuring that interactions on the website are performed by real people rather than automated programs.

For businesses, collecting Amazon data is vital for entering new markets and driving sales growth. However, as scraping efforts scale from a few pages to tens or more, CAPTCHAs can become a significant obstacle. In this article, we will explore an effective solution to solve CAPTCHAs while scraping Amazon product data, giving businesses a competitive advantage.

Amazon employs various CAPTCHA measures to protect the integrity of its website. Two common scenarios encountered when scraping Amazon are FunCaptcha and Image-to-Text CAPTCHAs.

FunCaptcha is a type of CAPTCHA technology developed by Arkose Labs. Unlike traditional CAPTCHAs, FunCaptcha uses interactive puzzles and games to differentiate between humans and bots. These puzzles are designed to engage and entertain humans while posing difficulties for bots.

On the other hand, Image-to-Text, also known as optical character recognition (OCR), is a technology that converts text within images into machine-readable text. It involves algorithms and computer vision techniques to analyze the visual patterns and structures of characters in an image and translate them into editable and searchable text.

To solve Amazon's CAPTCHAs effectively, many businesses rely on a specialized enterprise-level solution called [CapSolver](https://www.capsolver.com/). CapSolver is widely used in the market due to its high accuracy and speed. Here are the detailed steps for solving Amazon CAPTCHAs using CapSolver  
  
## Solving Amazon's Captchas with CapSolver

Capsolver, widely used in the market today, is an enterprise level specialized in solving Amazon CAPTCHA, as its high accuracy and fastness are chief in the market. Here are some detailed steps and details to solve Amazon CAPTCHA

### Solving Amazon Funcaptcha

#### Create Task

Create a task with the [createTask](../api-createtask.md) to create a task.

#### Task Object Structure

| Properties               | Type   | Required | Description                                                                                                                                                                                                                                                                                                                |
|--------------------------|--------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type                     | String | Required | `FunCaptchaTaskProxyLess`                                                                                                                                                                                                                                                                                                  |
| websiteURL               | String | Required | Web address of the website using funcaptcha, generally it's fixed value. (Ex: https://google.com)                                                                                                                                                                                                                          |
| websitePublicKey         | String | Required | The domain public key, rarely updated. (Ex: E8A75615-1CBA-5DFF-8031-D16BCF234E10)                                                                                                                                                                                                                                          |
| funcaptchaApiJSSubdomain | String | Optional | A special subdomain of [funcaptcha.com](http://funcaptcha.com/), from which the JS captcha widget should be loaded. Most FunCaptcha installations work from shared domains.                                                                                                                                                |
| data                     | String | Optional | Additional parameter that may be required by FunCaptcha implementation. Use this property to send "blob" value as a stringified array. See example how it may look like. {"\blob\":\"HERE_COMES_THE_blob_VALUE\"}  Learn [how to get FunCaptcha blob data](https://www.capsolver.com/blog/FunCaptcha/funcaptcha-data-blob) |
| proxy                    | String | Optional | Learn [Using proxies](../api-how-to-use-proxy)                                                                                                                                                                                                                                                                             |

#### Example Request

``` json
POST https://api.capsolver.com/createTask
Host: api.capsolver.com
Content-Type: application/json

{
    "clientKey": "YOUR_API_KEY_HERE",
    "task": {
        "type":"FunCaptchaTaskProxyLess", //Required
        "websiteURL":"", //Required
        "websitePublicKey":"", //Required
        "data": "{\"blob\": \"flaR60YY3tnRXv6w.l32U2KgdgEUCbyoSPI4jOxU...\"}" // Optional
    }
}
```


After you submit the task to us, you should receive in the response a 'Task id' if it's successfull. Please
read [errorCode: full list of errors](https://captchaai.atlassian.net/wiki/spaces/CAPTCHAAI/pages/394062/FuncaptchaTask+solving+FunCaptcha#)
if you didn't receive the task id.

#### Example Response

``` json
{
    "errorId": 0,
    "status": "idle",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006"
}

```

#### **Getting Result**

Use the [getTaskResult](../api-gettaskresult.md) method to get the recognition results

Depending on the system load, you will get the results within the interval of `1s` to `20s`

#### Example Request

``` json
POST https://api.capsolver.com/getTaskResult
Host: api.capsolver.com
Content-Type: application/json

{
    "clientKey": "YOUR_API_KEY",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006"
}
```

#### Example Response

``` json
{
    "errorId": 0,
    "solution": {
        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36",
        "token": "3AHJ_q25SxXT-pmSeBXjzScW-EiocHwwpwqtk1QXlJnGnU......"
    },
    "status": "ready"
}
```


### Solving Amazon Imagetotext



:::

#### Create Task

Create the task with the [createTask](../api-createtask.md).

**Task Object Structure**

Note that this type of task returns the task execution result directly after createTask, rather than getting it
asynchronously through getTaskResult.

| Properties | Type    | Required | Description                                                                                            |
|------------|---------|----------|--------------------------------------------------------------------------------------------------------|
| type       | String  | Required | ImageToTextTask                                                                                        |
| websiteURL | String  | Optional | Page source url to improve accuracy                                                                    |
| body       | String  | Required | base64 encoded content of the image (no newlines) (no data:image/****\*****; base64, content           |
| module     | String  | Optional | Specifies the module. Currently, the supported modules are common and queueit                          |
| score      | Float   | Optional | `0.8 ~ 1`, Identify the matching degree. If the recognition rate is not within the range, no deduction |
| case       | Boolean | Optional | Case sensitive or not                                                                                  |

#### Example Request

```text
POST https://api.capsolver.com/createTask
Host: api.capsolver.com
Content-Type: application/json
```

```json lines
{
  "clientKey": "YOUR_API_KEY",
  "task": {
    "type": "ImageToTextTask",
    "websiteURL": "https://xxxx.com",
    // You can choose the module you need to use
    // ocr single image model, default common
    "module": "queueit",
    // base64 encoded image
    "body": "/9j/4AAQSkZJRgABA......"
  }
}
```

#### Example Response

```json lines
{
  "errorId": 0,
  "errorCode": "",
  "errorDescription": "",
  "status": "ready",
  "solution": {
    "text": "44795sds"
  },
  "taskId": "2376919c-1863-11ec-a012-94e6f7355a0b"
}
```
