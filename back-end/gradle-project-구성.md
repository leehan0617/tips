## Gradle을 이용하여 spring boot 개발환경 구성하는방법 정리

### 개발 환경

1. spring boot : 2.x
2. jdk : 11
이 밖에 실제 프로젝트에서는 더많은 라이브러리 의존성을 갖게 되는데 필수적인 것만 우선 구성해보았음

## 프로젝트 세팅 방법

1. jdk 11을 이용하기 위해서는 intelliJ 최신 버전 (2018.3.3) 으로 했다.(이전 버전에서는 에러가발생하였음, 이클립스에서는 테스트 안해봄)
2. 로컬에 gradle을 설치 (현재 기준 5.1.1버전이 최신이다.) gradle은 homebrew를 통해 설치했다. (다른방법으로도 설치 가능) 터미널 및 cmd 에 gradle 버전을 확인할 수 있으면 OK
3. 최초 프로젝트 생성시에는 gradle이 지원해주는 키워드를 사용하여 terminal에서 프로젝트를 생성했다.
프로젝트 디렉토리를 생성후 그 디렉토리안에서 아래의 명령어를 실행하면 된다.
```
gradle init --type java-application
```
4. 프로젝트 생성 후 gradle 설정을 해야 하는데, IDE로 편집하는게 더 편하기 때문에 intelliJ를 킨 후 프로젝트 디렉토리를 불러온다.
5. IDE가 gradle 프로젝트인걸 인지하고, gradle 설정을 하라고 하는데 중요한건 IDE는 기본적으로 Use default gradle wrapper(recommended)에 체크되어있는데, 이걸 Use local gradle distribution으로 체크해야한다. (IDE gradle wrapper은 gradle 최신 버전에 맞춰져 있지 않고, IDE안에서 최적 버전에 맞춰져있는것 같다.)
6. local gradle distribution을 체크하게 되면, Gradle home을 지정해야하는데 homebrew로 설치했다면 gradle 경로는 /usr/local/Cellar/gradle/5.1.1/libexec 이다. (처음에 /usr/local/Cellar/gradle/5.1.1/bin 으로 했다가 에러가떳는데 libexec로 지정해야 에러가 발생 안함.)
7. Gradle JVM도 jdk 11을 선택한다.
8. OK를 누르면 프로젝트가 정상적으로 열린다.(열려야한다.)
9. build.gradle을 보면 gradle init으로 만들어서 기본적인 설정이 되어있는데 다 지운다.
10.build.gradle를 아래와 같이 수정
```
allprojects {
    apply plugin: 'java'
    apply plugin: 'application'
    apply plugin: 'idea'
    apply plugin: 'eclipse'
   
    ext {
        springBootVersion = '2.1.2.RELEASE'
    }

    sourceCompatibility = JavaVersion.VERSION_11
    targetCompatibility = JavaVersion.VERSION_11

    repositories {
        mavenCentral()
    }

    dependencies {
         compile "org.springframework.boot:spring-boot:${springBootVersion}"
         compile "org.springframework.boot:spring-boot-starter:${springBootVersion}"
         compile "org.springframework.boot:spring-boot-starter-web:${springBootVersion}"

         testCompile "org.springframework.boot:spring-boot-starter-test:${springBootVersion}"
    }
}
```  
11. build를 돌려보고 스프링 관련 라이브러리가 생성되었으면 끝.
12. 추가적인 설정이 필요할 수 있는데 gradle 관련 예제 및 공식사이트를 참조해서 추가하면 된다.
