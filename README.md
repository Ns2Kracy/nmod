# 自建应用商店源使用

自建商店源是一个可公开访问的 JSON 文件，里面声明模块名称、版本和 `.raw` 包下载地址。设备把这个 JSON 地址写入 `mod-management.conf` 后，就可以用 `zpkg install <module_name>` 安装模块。

## 搭建商店源

创建 `mod-store.json`：

```json
[
  {
    "name": "example-module",
    "title": "Example Module",
    "version": "v1.0.0",
    "url": "https://example.com/modules/example-module/v1.0.0/example-module.raw"
  }
]
```

字段说明：

| 字段 | 说明 |
| --- | --- |
| `name` | 模块名，安装时使用 `zpkg install <name>`。 |
| `title` | 展示名称。 |
| `version` | 模块版本。 |
| `url` | `.raw` 包下载地址。 |

把 `mod-store.json` 和 `.raw` 包放到可公开访问的 HTTP/HTTPS 地址上，例如：

```text
https://example.com/mod-store.json
https://example.com/modules/example-module/v1.0.0/example-module.raw
```

## 配置商店源

编辑 `/etc/casaos/mod-management.conf`，把 `[server]` 段的 `modstore` 改成自建商店源地址：

```ini
[common]
RuntimePath = /var/run/casaos

[app]
LogPath = /var/log/casaos/
LogSaveName = mod-management
LogFileExt = log

[server]
modstore = https://example.com/mod-store.json
```

如果文件里已经有 `[server]` 段，只需要修改 `modstore` 这一行。

## 安装模块

配置完成后，直接使用商店源条目里的 `name` 安装：

```bash
zpkg install example-module
```

如果安装失败，先确认 `modstore` 地址能访问，并且条目里的 `url` 能直接下载 `.raw` 包。
