# 多个 FROM

Dockerfile 中, 大多数指令会生成一个层

FROM: node 基础镜像中已经存在很多层

多个 FROM 最后的 FORM 为准, 其他的被抛弃
可以将前面阶段的构建产物 应用到最后一个 FROM

```bash
# 编译阶段
FROM golang:1.10.3

COPY server.go /build/

WORKDIR build

RUN GOARM=6 go build -ldflags '-w -s' -o server

# 运行阶段
FROM scratch  最小化镜像

# 把编译阶段的构建产物 拷贝到当前镜像中  0表示第一个from  FROM xxx as build
COPY --from=0 /build/server /

ENTRYPOINT ["/server"]
```

COPY --from 还可以直接拷贝镜像中的文件
COPY --from=quay.io/coreos/etcd:v3.9 /usr/local/bin/etcd /usr/local/bin

```Dockerfile
# build environment
FROM node:13.12.0-alpine as build
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH
COPY package.json ./
COPY package-lock.json ./
RUN npm ci --silent
RUN npm install react-scripts@3.4.1 -g --silent
COPY . ./
RUN npm run build

# production environment
FROM nginx:stable-alpine
COPY --from=build /app/build /usr/share/nginx/html
# new
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

```nginx
server {

  listen 80;

  location / {
    root   /usr/share/nginx/html;
    index  index.html index.htm;
    try_files $uri $uri/ /index.html;
  }

  error_page   500 502 503 504  /50x.html;

  location = /50x.html {
    root   /usr/share/nginx/html;
  }

}
```
