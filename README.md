# FolioSpace Library for LazyCat

本仓库将 [FolioSpace Library](https://foliospace.app/zh) 的官方 Docker 镜像打包为懒猫微服 LPK，并通过 GitHub Actions 跟随稳定镜像版本自动发布到喵喵商店。

FolioSpace Library 是一款自托管的个人数字资产库，可将用户自己拥有的电子书、漫画、PDF、ROM、视频和其他文档整理为统一索引，并通过 Web、HTTP API、MCP 与 Apple 客户端访问。本应用不包含或分发任何媒体资源。

## LazyCat 集成

- 多实例部署：每位用户拥有独立容器、配置数据库和访问令牌，不需要选择用户 UID。
- 媒体隔离：`library`、`books`、`games` 和 `videos` 位于当前用户的应用文稿目录，并以只读方式挂载。
- 数据归属：SQLite、缩略图、缓存和内部状态位于应用持久目录；用户媒体在卸载应用后默认保留。
- 登录体验：部署时生成访问令牌，登录页自动填充并提交；首次初始化页只自动填充，不会代替用户选择媒体目录。
- 文件集成：内置懒猫文件选择器拦截脚本。
- 镜像模式：运行镜像通过镜像加速地址交付，并要求与上游目标架构 digest 一致。

## 首次使用

首次打开应用时，在初始化页面选择 `/library`、`/books`、`/games` 或 `/videos` 作为首个资源目录，然后完成初始化。其他目录可稍后在 FolioSpace 中继续添加。

## 自动发布

工作流每天检查 `docker.io/funland/foliospace-library` 的稳定版本。上游使用两段版本号（如 `0.994`），LPK 会规范化为三段 SemVer（如 `0.994.0`）。发布仅面向喵喵商店，不启用懒猫官方商店。

必需的 GitHub Secrets：

- `APPSTORE_URL`
- `APPSTORE_TOKEN`

可选的 GitHub Secrets：

- `APP_ID`
- `PRIVATE_STORE_GROUP_CODES`

组织级 Secrets 必须授权此仓库。同名 Repository Secret 会覆盖 Organization Secret。

## 本地验证

```bash
lzc-cli project release -o dist/foliospace-library.lpk
lzc-cli lpk info dist/foliospace-library.lpk
```

上游项目由 funland 维护并以 AGPL-3.0 许可发布。
