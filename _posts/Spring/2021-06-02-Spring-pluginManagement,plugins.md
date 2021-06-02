---
title: "Spring : pluginManagement와 plugins의 차이와 주의할 점"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : plugins"
  - "Spring : pluginManagement"
toc: true
toc_sticky: true
toc_label: 목차
---

#  pluginManagement와 plugins의 차이와 주의할 점

`MapStruct`에 관한 Maven 의존성 설정 후, 테스트하려는데 오류가 계속 나서 의존성 설정에 관한 구글링을 했지만 해결되지 않았습니다.. ㅠㅠ 그러다 혹시나 해서 `pom.xml`파일의 plugins 부분을 살펴보다 오류를 해결하였는데, 오류의 원인은 `pluginManagement`와 `plugins`의 차이 때문이었습니다.



## 차이점

`<pluginManagement> </pluginManagement>`와 `<plugins> </plugins>` 둘 다 plugin에 대한 설정을 할 수 있지만 큰 차이점이 존재합니다.

- `<pluginManagement> </pluginManagement>`  :여러 모듈들간에 설정 내용을 공유할 때 사용(Ex: parent-pom 에 지정후 child-pom 에서 상속받아 사용)

- `<plugins> </plugins>` : 해당 파일에서만 사용



### 예시

```xml
<!-- 부모 pom.xml -->
<pluginManagement>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-jar-plugin</artifactId>
			<version>2.2</version>
			<executions>
				<execution>
					<id>pre-process-classes</id>
					<phase>compile</phase>
					<goals>
						<goal>jar</goal>
					</goals>
					<configuration>
						<classifier>pre-process</classifier>
					</configuration>
				</execution>
			</executions>
		</plugin>
	</plugins>
</pluginManagement>
```

부모 pom에서 의존성 설정을 `<pluginManagement> </pluginManagement>`에 넣어주었다고 했을 때, 자식 pom.xml은 다음과 같이 간단히 사용할 수 있습니다.



```xml
<!-- 자식 pom.xml -->
<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
    </plugin>
</plugins>
```

자식 pom의 `<plugins> </plugins>`에서 부모 pom의 `<pluginManagement> </pluginManagement>`설정을 가져와 사용할 수 있습니다.



## 주의할 점

위의 사례로 부모 pom의 설정을 자식 pom이 상속받아 사용할 수 있다는 것을 알 수 있는데, 이 말은 **parent, child 구성의 pom 이 아니라면 굳이 pluginManagement 를 쓸 필요가 없다는 것**입니다.

또, 한 가지 명심해야할 것은 **pluginManagement에 들어있는 plugin은 해당 프로젝트에서 작동하지 않는다**는 것입니다.

이 문제 때문에 MapStruct 의존성 설정 시 문제가 되었고, pluginManagement를 빼주니 문제가 해결되었습니다.



## 정리

- `pluginManagement`와 `plugins` 둘 다 설정을 할 수 있지만, **pluginManagement는 여러 모듈들간에 설정 내용을 공유할 때 사용**하고, **plugins는 해당 파일에서만 사용**합니다.
- `pluginManagement`는 parent, child 구성의 pom이 아니라면 굳이 쓸 필요가 없습니다.
- `pluginManagement`에 들어있는 plugin은 해당 프로젝트에서 작동하지 않습니다.



---

참고 : [https://www.lesstif.com/java/maven-plugins-pluginmanagement-18220236.html](https://www.lesstif.com/java/maven-plugins-pluginmanagement-18220236.html)

[https://devday.tistory.com/entry/%EB%A9%94%EC%9D%B4%EB%B8%90-Maven-pluginManagement-vs-plugins](https://devday.tistory.com/entry/%EB%A9%94%EC%9D%B4%EB%B8%90-Maven-pluginManagement-vs-plugins)

[https://velog.io/@hyoputer/pluginManagement-vs-plugins](https://velog.io/@hyoputer/pluginManagement-vs-plugins)

