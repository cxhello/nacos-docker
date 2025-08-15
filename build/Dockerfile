FROM eclipse-temurin:8-jre-jammy

LABEL maintainer="cxhello <caixiaohuichn@gmail.com>"

# 环境变量
ENV MODE="cluster" \
    PREFER_HOST_MODE="ip" \
    BASE_DIR="/home/nacos" \
    CLASSPATH=".:/home/nacos/conf:$CLASSPATH" \
    CLUSTER_CONF="/home/nacos/conf/cluster.conf" \
    FUNCTION_MODE="all" \
    JAVA_HOME="/opt/java/openjdk" \
    NACOS_USER="nacos" \
    JAVA="/opt/java/openjdk/bin/java" \
    JVM_XMS="1g" \
    JVM_XMX="1g" \
    JVM_XMN="512m" \
    JVM_MS="128m" \
    JVM_MMS="320m" \
    NACOS_DEBUG="n" \
    TOMCAT_ACCESSLOG_ENABLED="false" \
    TIME_ZONE="Asia/Shanghai"

ARG NACOS_VERSION=2.2.3
ARG HOT_FIX_FLAG=""

WORKDIR $BASE_DIR

# 安装依赖 + 下载 Nacos + 初始化目录和权限
RUN set -x \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
       curl \
       iputils-ping \
       netcat \
       vim \
    && rm -rf /var/lib/apt/lists/* \
    \
    && curl -SL "https://github.com/alibaba/nacos/releases/download/${NACOS_VERSION}${HOT_FIX_FLAG}/nacos-server-${NACOS_VERSION}.tar.gz" -o /home/nacos-server.tar.gz \
    && tar -xzvf /home/nacos-server.tar.gz -C /home \
    && rm -rf /home/nacos-server.tar.gz \
              /home/nacos/bin/* \
              /home/nacos/conf/*.properties \
              /home/nacos/conf/*.example \
              /home/nacos/conf/nacos-mysql.sql \
    \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime \
    && echo $TIME_ZONE > /etc/timezone \
    \
    && mkdir -p logs \
    && touch logs/start.out \
    && ln -sf /dev/stdout start.out \
    && ln -sf /dev/stderr start.out

# 复制启动脚本与配置
COPY bin/docker-startup.sh bin/docker-startup.sh
COPY conf/application.properties conf/application.properties

RUN chmod +x bin/docker-startup.sh

EXPOSE 8848
ENTRYPOINT ["bin/docker-startup.sh"]