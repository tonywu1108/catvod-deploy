# catvod-deploy

CatVodSpider 的 GitHub 托管目录，只包含运行所需的静态文件。

仓库：https://github.com/tonywu1108/catvod-deploy

## 目录内容

```text
.
├── config.json
├── .nojekyll
└── spider/
    ├── custom_spider_v1.jar
    └── custom_spider_v1.jar.md5
```

## 用户配置地址（推荐）

```text
https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/config.json
```

如果 raw 无法访问，可换成 GitHub Pages：

```text
https://tonywu1108.github.io/catvod-deploy/config.json
```

## 访问地址

| 用途 | 地址 |
| --- | --- |
| 配置（Raw） | `https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/config.json` |
| 配置（Pages） | `https://tonywu1108.github.io/catvod-deploy/config.json` |
| JAR（Raw） | `https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/spider/custom_spider_v1.jar` |
| JAR（Pages） | `https://tonywu1108.github.io/catvod-deploy/spider/custom_spider_v1.jar` |
| JAR（jsDelivr） | `https://cdn.jsdelivr.net/gh/tonywu1108/catvod-deploy@main/spider/custom_spider_v1.jar` |

> `config.json` 里的 `spider` 当前指向 **Raw** 地址，实测可直接下载且 MD5 一致。
> 若用户所在网络访问不了 raw，可改为 Pages 或 jsDelivr 地址，注意同步修改 MD5。

## 验证

```powershell
# 配置
Invoke-WebRequest "https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/config.json" -UseBasicParsing

# JAR 大小与 MD5
Invoke-WebRequest "https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/spider/custom_spider_v1.jar" -OutFile "$env:TEMP\check.jar" -UseBasicParsing
(Get-Item "$env:TEMP\check.jar").Length
(Get-FileHash -Algorithm MD5 "$env:TEMP\check.jar").Hash.ToLowerInvariant()
```

期望：

```text
188877
7f0afad1edd2892b51e412295450e66e
```

## 更新 JAR

1. 重新编译 `CatVodSpider`，得到新的 `custom_spider.jar`。
2. 复制为 `spider/custom_spider_v2.jar`（建议每次换版本号）。
3. 更新 `config.json` 中的 `spider` 地址和 `;md5;`。
4. 提交并推送：

```powershell
git add .
git commit -m "update spider jar to v2"
git push
```

5. 等 1~2 分钟让 Raw / Pages 生效。

## 注意

- 仓库必须是 Public，否则 Raw / Pages 都无法匿名访问。
- JAR 是任意 Java 代码，只部署可信文件。
- 不要在本仓库放账号密码、Cookie、OSS 密钥。
