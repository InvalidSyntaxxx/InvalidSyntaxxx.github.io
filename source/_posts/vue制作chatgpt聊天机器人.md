---
title: 制作网页ChatGPT聊天机器人
tags:
  - Vue
id: '1218'
categories:
  - - 学习笔记
date: 2023-03-10 18:35:54
---

单页面实现ChatGPT聊天机器人，给你的网站加点活力！
<!-- more -->
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
</head>

<body>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>

    <div id="app" style="display: flex;flex-flow: column;margin: 20 ">
        <div style="display: flex;justify-content: center;align-items:
                center;margin: 20">
            <textarea type="text" v-model="info" cols="40" rows="3" style="margin:15"></textarea>
            <button v-on:click="ask()">发送</button>
        </div>
        <div style="display: flex;justify-content: center;align-items:
                center;">
            <textarea name="res" id="res" cols="100" rows="20">{{res}}</textarea>
        </div>
    </div>
    <script>
        const {createApp} = Vue
        createApp({
            data() {
                return {
                    MAX_TOKEN: 4096,
                    total: 0,
                    info: '如果一个面包发臭了，应该怎么办？',
                    messages: [],
                    res: '',
                    api: '你的OpenAI key'
                }
            },
            methods: {
                ask() {
                    if (!this.info) {
                        return;
                    } else {
                        this.messages.push({"role": "user", "content": this.info})
                        this.total++;
                    }
                    // 限制max_token，大于5个问题时删除首元素
                    while (this.total.length > 1) {
                       this.total = this.tota - 1;
                        this.messages.shift()
                    }

                    this.res = '请求中...'
                    axios.post('https://api.openai.com/v1/chat/completions', {
                        messages: this.messages,
                        max_tokens: this.MAX_TOKEN / 2,
                        model: "gpt-3.5-turbo-0301"  //选择最新的模型，能支持到6月1号
                    }, {
                        headers: {'content-type': 'application/json', 'Authorization': 'Bearer ' + this.api}
                    }).then(response => {
                        this.res = response.data['choices'][0]['message']['content'];
                        this.messages.push(response.data['choices'][0]['message']);
                        this.total ++;
                    })
                }
            }
        }).mount('#app')
    </script>
</body>

</html>
```

缺点：

1.  外网IP+国外手机号
2.  命令限制，只能最高4096tokens

所以，我自己开发了一个聊天室，支持移动端和PC端。单独聊天或群聊都可以和ChatGPT聊天 地址：[https://chat.wangwangyz.site/](https://chat.wangwangyz.site/) ![](https://www.wangwangyz.site/个人图床/image-20230310223018574.png) ![image-20230310223018574](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20230310223018574.png)