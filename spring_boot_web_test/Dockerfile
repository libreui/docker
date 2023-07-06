# 以jdk17为基础镜像
FROM mdsol/java17-jdk
# 描述
LABEL description="SpringBootWeb Java 17"

# 拷贝jar包到容器中
COPY springboot-study-0.0.1-SNAPSHOT.jar /usr/local

# 容器工作目录
WORKDIR /usr/local

# 设置字符集
ENV LANG C.UTF-8

# 运行jar包
ENTRYPOINT ["java", "-jar","/usr/local/springboot-study-0.0.1-SNAPSHOT.jar"]