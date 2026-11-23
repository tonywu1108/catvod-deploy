# catvod-deploy

CatVodSpider 的 GitHub 托管目录，只包含运行所需的静态文件。

## 目录内容

```text
.
├── config.json
├── .nojekyll
└── spider/
    ├── custom_spider_v1.jar
    └── custom_spider_v1.jar.md5
```

## 第一次发布

1. 在 GitHub 新建一个 **Public** 仓库，名字例如 `catvod-deploy`，不要勾选初始化 README。
2. 在本目录执行：

```powershell
git remote add origin https://github.com/tonywu1108/catvod-deploy.git
git push -u origin main
```

> 如果用户名或仓库名不是 `tonywu1108/catvod-deploy`，先全局替换
> `config.json` 里的地址，并修改上面的远程地址。

## 访问地址

推送成功后：

| 用途 | 地址 |
| --- | --- |
| 配置（Raw） | `https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/config.json` |
| 配置（jsDelivr） | `https://cdn.jsdelivr.net/gh/tonywu1108/catvod-deploy@main/config.json` |
| JAR（jsDelivr） | `https://cdn.jsdelivr.net/gh/tonywu1108/catvod-deploy@main/spider/custom_spider_v1.jar` |
| JAR（Raw） | `https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/spider/custom_spider_v1.jar` |

推荐给用户填写的配置地址：

```text
https://raw.githubusercontent.com/tonywu1108/catvod-deploy/main/config.json
```

国内访问慢时改用 jsDelivr 配置地址。

## 开启 GitHub Pages（可选）

仓库 Settings → Pages：

- Source: `Deploy from a branch`
- Branch: `main`
- Folder: `/ (root)`

之后可用：

```text
https://tonywu1108.github.io/catvod-deploy/config.json
https://tonywu1108.github.io/catvod-deploy/spider/custom_spider_v1.jar
```

新仓库没有 CNAME，不会和原仓库的 `fm.t4tv.hz.cz` 冲突。

## 更新 JAR

1. 重新编译项目，得到新的 `custom_spider.jar`。
2. 复制为 `spider/custom_spider_v2.jar`（建议每次都换版本号）。
3. 更新 `config.json` 中的 `spider` 地址和 `;md5;`。
4. 提交并推送：

```powershell
git add .
git commit -m "update spider jar to v2"
git push
```

5. jsDelivr 有缓存，可访问以下地址刷新：

```text
https://purge.jsdelivr.net/gh/tonywu1108/catvod-deploy@main/spider/custom_spider_v2.jar
```

## 验证

```powershell
curl.exe -I "https://cdn.jsdelivr.net/gh/tonywu1108/catvod-deploy@main/spider/custom_spider_v1.jar"
curl.exe -L "https://cdn.jsdelivr.net/gh/tonywu1108/catvod-deploy@main/spider/custom_spider_v1.jar" -o "$env:TEMP\check.jar"
(Get-FileHash -Algorithm MD5 "$env:TEMP\check.jar").Hash.ToLowerInvariant()
```

应与 `config.json` 中的 MD5 一致：

```text
7f0afad1edd2892b51e412295450e66e
```

## 注意

- 仓库必须是 Public，否则 Raw / jsDelivr / Pages 都无法匿名访问。
- JAR 是任意 Java 代码，只部署可信文件。
- 不要在本仓库放账号密码、Cookie、OSS 密钥。
