# ChatGPTSummary

一个用于总结选中文字的 PopClip 插件

修改于 https://reorx.com/makers-daily/003-chatgpt-proofreader-extension-popclip/

选中下面的代码

```js
// #popclip extension for ChatGPT
// name: ChatGPT Summary
// icon: iconify:fluent:calligraphy-pen-24-regular
// language: javascript
// module: true
// entitlements: [network]
// options: [{
//   identifier: apikey, label: API Key, type: string,
//   description: 'Obtain API key from https://platform.openai.com/account/api-keys'
// }]
async function chat(input, options, lang) {
  const openai = require("axios").create({
    baseURL: "https://api.openai.com/v1",
    headers: { Authorization: `Bearer ${options.apikey}` },
  });

  let messages;
  switch (lang) {
    case "en":
      messages = [
        {
          role: "user",
          content: `Summarize the following text information:\n${input.text}`,
        },
      ];
      break;
    case "zh":
      messages = [
        {
          role: "user",
          content: `总结下面的文本信息:\n${input.text}`,
        },
      ];
      break;
  }

  const { data } = await openai.post("/chat/completions", {
    model: "gpt-3.5-turbo",
    messages,
  });
  const result = data.choices[0].message;
  return result.content.trim();
}

exports.actions = [
  {
    title: "ChatGPT: summary zh",
    icon: "square filled 总",
    after: "show-result",
    code: async (input, options) => chat(input, options, "zh"),
  },
];

```

![fGi06N](https://cdn.jsdelivr.net/gh/klaaay/pbed@main/uPic/fGi06N.jpg)