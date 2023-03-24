FROM elasticsearch:7.17.0

# RUN sed -i 's/security.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list && \
#     sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list

# 更新软件源
RUN apt-get update && \
    apt-get install -y vim wget gpg

# 安装kibana 从apt软件源安装
# 参考 https://www.elastic.co/guide/en/kibana/7.17/deb.html#deb-repo
# RUN wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
# RUN apt-get install -y apt-transport-https
# RUN echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | tee /etc/apt/sources.list.d/elastic-7.x.list
# RUN apt-get update && apt-get install -y kibana


# 安装中文分词  https://github.com/medcl/elasticsearch-analysis-ik
# 注意：分词版本需和es版本一致！此步安装需要进入到容器之后执行，因为安装过程中有确认环节，会报错
# RUN elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.17.0/elasticsearch-analysis-ik-7.17.0.zip
