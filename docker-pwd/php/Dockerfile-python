# 基础镜像默认安装了python3
FROM python:3-alpine3.6

# LABEL maintainer "https://github.com/hermsi1337"

# 复制添加必要的文件
COPY requirements.txt ./
COPY entrypoint.sh /usr/local/bin/

# 默认的环境变量，ROOT_PASSWORD的默认值不要修改
ENV ROOT_PASSWORD root

# 安装必要的命令行工具
RUN  apk --update add \
        libressl \
        ca-certificates \
        rsync \
        vim \
        git \
        curl \
        wget \
        gzip \
        tar \
        patch \
        mariadb-client

# 安装 openssh
RUN apk --update add openssh \
		&& sed -i s/#PermitRootLogin.*/PermitRootLogin\ yes/ /etc/ssh/sshd_config \
		&& echo "root:${ROOT_PASSWORD}" | chpasswd \
		&& rm -rf /var/cache/apk/* /tmp/*

# 安装要求的 python 库    
RUN pip install --no-cache-dir -r requirements.txt

# 设置当前工作目录为python应用目录
WORKDIR /usr/src/app

# 暴露端口
EXPOSE 22

# 设置入口文件
RUN  chmod +x /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
