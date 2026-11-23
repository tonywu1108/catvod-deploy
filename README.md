# catvod-deploy

CatVodSpider 的 GitHub 托管目录，只包含运行所需的静态文件。

仓库：https://github.com/tonywu1108/catvod-deploy
站点：https://tonywu1108.github.io/catvod-deploy/

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
https://tonywu1108.github.io/catvod-deploy/config.json
```

备选（可能受网络影响）：

```text
https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/config.json
https://cdn.jsdelivr.net/gh/tonywu1108/catvod-deploy@main/config.json
```

## 访问地址

| 用途 | 地址 |
| --- | --- |
| 配置（Pages） | `https://tonywu1108.github.io/catvod-deploy/config.json` |
| 配置（Raw） | `https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/config.json` |
| JAR（Pages） | `https://tonywu1108.github.io/catvod-deploy/spider/custom_spider_v1.jar` |
| JAR（Raw） | `https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/spider/custom_spider_v1.jar` |
| JAR（jsDelivr） | `https://cdn.jsdelivr.net/gh/tonywu1108/catvod-deploy@main/spider/custom_spider_v1.jar` |

> `config.json` 里的 `spider` 当前指向 **GitHub Pages**。
> 若 Pages 访问不了，可改成上表中的 Raw 或 jsDelivr 地址，并同步更新 MD5。
> Raw 地址有 CDN 缓存，推送后可能需要几分钟才更新。

## 验证

```powershell
# 配置
(Invoke-WebRequest "https://tonywu1108.github.io/catvod-deploy/config.json" -UseBasicParsing).Content

# JAR 大小与 MD5
Invoke-WebRequest "https://tonywu1108.github.io/catvod-deploy/spider/custom_spider_v1.jar" -OutFile "$env:TEMP\check.jar" -UseBasicParsing
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

5. 等 Pages 重新构建（约 1 分钟），在 Actions 里看到 `pages build and deployment` 变绿即可。

## 注意

- 仓库必须是 Public，否则 Raw / Pages 都无法匿名访问。
- JAR 是任意 Java 代码，只部署可信文件。
- 不要在本仓库放账号密码、Cookie、OSS 密钥。
