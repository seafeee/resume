# Resume

一个基于 React 的在线 Markdown 简历编辑与导出项目，支持在浏览器中编辑简历内容，并导出为 HTML 或 PDF。

这个仓库基于开源项目 `resumd` 修改，已经补充了适配当前本机 Node 环境的启动和打包脚本，方便本地开发和部署到云服务器。

## 功能特点

- 在线编辑 Markdown 简历
- 实时预览简历内容
- 支持导出 HTML / PDF
- 支持主题样式切换
- 可打包为静态站点部署到 Nginx

## 本地开发

### 安装依赖

```bash
npm install
```

### 启动项目

```bash
npm start
```

项目默认启动后可通过下面地址访问：

```text
http://127.0.0.1:3000
```

说明：

- 当前仓库已经在 `start` 脚本中加入 `--openssl-legacy-provider`
- 这是为了兼容旧版 `react-scripts` 与较新的 Node.js 环境

## 生产打包

执行下面命令生成生产环境静态文件：

```bash
npm run build
```

打包完成后，产物目录为：

```text
build/
```

如果你需要上传部署，可以直接使用 `build` 目录，或自行压缩后上传到云服务器。

## 部署到 Nginx

这是一个纯前端静态项目，推荐部署方式是：

1. 将 `build` 目录上传到服务器
2. 放到站点目录，例如 `/var/www/resume/build`
3. 在 Nginx 中将站点根目录指向该目录

示例配置：

```nginx
server {
    listen 80;
    server_name _;

    root /var/www/resume/build;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /static/ {
        expires 30d;
        add_header Cache-Control "public, immutable";
        try_files $uri =404;
    }
}
```

应用配置前可执行：

```bash
sudo nginx -t
sudo systemctl reload nginx
```

## 项目结构

```text
src/            前端源码
public/         公共静态资源
samplePDFs/     示例导出文件
build/          生产构建产物
```

## 常用命令

```bash
npm start
npm run build
npm test
```

## 说明

- 本仓库当前更适合作为个人部署和二次修改使用
- 如果后续升级 React 或 `react-scripts`，可以再去掉兼容参数
- 如需接入自己的域名，可在 Nginx 中配置 `server_name`

## 致谢

原始项目来源：

- [timqian/resumd](https://github.com/timqian/resumd)
