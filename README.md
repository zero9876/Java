### 环境准备，下载java、maven

```bash
sudo yum install -y java-1.8.0-openjdk-devel maven

mvn -version | grep "Apache Maven" && java -version | grep "1.8.0"
```

### 创建项目目录结构和Java应用示例

```bash
mkdir -p ~/my-java-app/src/main/java/hello

cat <<EOF > ~/my-java-app/src/main/java/hello/HelloWorld.java
package hello;
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Container World!");
    }
}
EOF

ls ~/my-java-app/src/main/java/hello/HelloWorld.java
```

<h3>添加 Maven 构建配置


```bash
cat <<EOF > ~/my-java-app/pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>my-java-app</artifactId>
    <version>1.0-SNAPSHOT</version>
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
</project>
EOF

cat ~/my-java-app/pom.xml | grep '<artifactId>my-java-app</artifactId>'
```

<h3>构建 Java 应用


```bash
cd ~/my-java-app && mvn clean package

ls target/classes/hello/HelloWorld.class
```

<h3>创建 Dockerfile


```bash
cat <<EOF > ~/my-java-app/Dockerfile
FROM openjdk:8-jre
WORKDIR /app
COPY target/classes /app/classes
CMD ["java", "-cp", "/app/classes", "hello.HelloWorld"]
EOF

cat ~/my-java-app/Dockerfile | grep 'hello.HelloWorld'
```

<h3>构建 Docker 镜像


```bash
cd ~/my-java-app && docker build -t my-java-app .

docker images | grep my-java-app
```

<h3>运行容器


```bash
docker run --name myapp my-java-app

docker logs myapp | grep 'Hello, Container World!'
```

