# Maven入门和进阶

## Maven

- Maven是一款为JAVA项目进行管理的软件类似于Node中的npm，可以对包进行管理，对依赖进行安装，对项目进行打包
- 构建时，每种开发工具的构建结构是不一样的，IDEA和eclipse的文件结构就不一致，而一般使用统一的Maven结构进行构建，

## Maven安装-配置

-  首先去官网下载，注意要下载和本级JDK对应的Maven版本，下载后将包解压在一个文件夹中
- 然后配置一个MAVEN_HOME的环境变量，然后在path变量中加入MAVEN_HOME对应的bin目录
- 这样即可配置完成，测试是否配置成功，需要使用mvn -v命令，如果弹出正确的版本，即代表安装成功
- 配置，在maven/conf/setting.xml中修改
  - 因为MAVEN会在本地生成一个MAVEN仓库，存放了本地的jar包，默认放在C盘中，所以需要改为D盘
  - 配置镜像环境，由于默认的服务器在国外，下包时，会有速度过慢的情况，所以需要配置一个阿里镜像
  - 配置对应的jdk版本
- 在idea中配置刚才设置的maven即可

# Spring MVC

# MyBatis

# Spring Boot

# MyBatis-Plus