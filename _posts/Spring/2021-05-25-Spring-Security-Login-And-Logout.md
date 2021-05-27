---
title: "Spring : Spring Security를 이용한 로그인 및 로그아웃"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Spring Security"
toc: true
toc_sticky: true
toc_label: 목차
---

# Spring Security를 이용한 로그인 및 로그아웃

[Spring Security 포스트](https://dev-splin.github.io/spring/Spring-Security/)에서 Spring Security 설정을 해봤기 때문에 이어서, **Spring Security를 이용한 로그인 및 로그아웃**에 대해 알아보려고 합니다.



## PasswordEncoder 테스트하기

`PasswordEncoder`객체를 `빈(Bean)`으로 등록했었습니다. 그럼, 해당 빈을 테스트해보도록 하겠습니다. 해당 빈을 테스트하는 이유는 아이디/암호를 입력해서 로그인 처리를 하려면 반드시 암호가 인코딩(encoding)되어 있어야 하기 때문입니다.

src/test/java 폴더에 다음과 같이 PasswordEncoderTest클래스를 작성합니다.



![img](https://cphinf.pstatic.net/mooc/20200420_271/1587373703133iY9N0_PNG/mceclip0.png)



```java
package org.edwith.webbe.securityexam.service;

import org.edwith.webbe.securityexam.config.ApplicationConfig;
import org.edwith.webbe.securityexam.config.SecurityConfig;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {ApplicationConfig.class, SecurityConfig.class})
public class PasswordEncoderTest {
	
	@Autowired
	PasswordEncoder passwordEncoder;
	
	@Test
	public void passwordEncode() throws Exception {
		System.out.println(passwordEncoder.encode("1234"));
	}
	
	@Test
	public void passwordTest() throws Exception {
		// passwordEncoder의 encode() 메서드를 이용해서 인코딩한 값, 인코딩할 때 마다 값이 바뀝니다.
		String encodePasswd = "$2a$10$USbExG2YOZJqu5rR9eWAqO3NqwjS6c8uI0c695cnURA2gxqRnx41O";
		String password = "1234";
		// 인코딩한 값과 원래의 값이 같은지를 검사해 줍니다.
		// 인코딩할 때마다 값이 바뀌어도 결국 1234를 표현한 값이기 때문에 true가 나옵니다.
		boolean test = passwordEncoder.matches(password, encodePasswd);
		System.out.println(test);
	}

}
```

위의 코드를 보면 2개의 테스트 메소드를 구현하고 있습니다.

`passwordEncode()` 테스트 메소드에서는 `passwordEncode`의 `encode()`메소드를 이용해 문자열 "1234"를 인코딩한 후 출력하고 있습니다.

테스트 메소드에서는 assert메소드를 이용해 검사를 해주어야하는데 여기에서는 개발자가 직접 눈으로 확인하기 위해서 `System.out.println()`으로 출력하고 있습니다.

이 때 주의해야할 점은 **실행할 때마다 다른 결과**가 나온다는 것입니다. 그리고, **passwordEncode로 인코딩된 문자열을 원래의 문자열로 바꿀 수 있는 방법은 존재하지 않습니다. 이런 방식을 단방향 암호화**라고 합니다.



인코딩된 문자열이 그럼 "1234"에 의해 인코딩 된 건지는 어떻게 알 수 있을까요?
이를 위해 `PasswordEncode`는 `matches()`라는 메소드를 제공합니다.
`passwordEncoder.matches("1234","$2a$10$USbExG2YOZJqu5rR9eWAqO3NqwjS6c8uI0c695cnURA2gxqRnx41O")` 와 같이 실행했을 때 결과가 true가 나오게 되면 `"$2a$10$USbExG2YOZJqu5rR9eWAqO3NqwjS6c8uI0c695cnURA2gxqRnx41O"`는 `"1234"`가 인코딩된 문자열이라는 것을 의미합니다.

**matches() 메소드는 사용자가 아이디와 암호를 입력했을 때 이 암호가 맞는지 검사**를 하게 되는데, **스프링 시큐리티는 내부적으로 matches() 메소드를 이용해서 검증을 수행**합니다. 사용자가 입력한 문자열과 암호화된 문자열을 비교해서 검증을 하게 되는 것이죠. 그렇기 때문에 암호는 인코딩 된 형태로 저장이 되어 있어야 합니다.



## 로그인/로그아웃 처리를 위한 설정 수정하기

기존의 `SecurityConfig.java` 파일을 다음과 같이 수정합니다.

코드에 있는 `CustomUserDetailsService`는 일단 설정 부분을 본 후에 만들도록 하겠습니다.

```java
package org.edwith.webbe.securityexam.config;

import org.edwith.webbe.securityexam.service.security.CustomUserDetailsService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.builders.WebSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
	@Autowired
	CustomUserDetailsService customUserDetailsService;

    //   /webjars/** 경로에 대한 요청은 인증/인가 처리하지 않도록 무시합니다.
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers(
                "/webjars/**");
    }
    

    @Override
    // AuthenticationFilter는 아이디/암호를 입력해서 로그인할 때 처리해주는 필터
    // 아이디에 해당하는 정보를 DB에서 읽어 들일 때 UserDetailsService를 구현하고 있는 객체를 이용
    // customUserDetailsService는 userDetailsService를 구현하고 있는 객체입니다.
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    	auth.userDetailsService(customUserDetailsService);
	}



	//   /, /main에 대한 요청은 누구나 할 수 있지만, 
	//   그 외의 요청은 모두 인증 후 접근 가능합니다.
    @Override
    protected void configure(HttpSecurity http) throws Exception {
    	 http
    	 // csrf를 사용하지 않습니다. disable() 메소드는 http를 반환합니다.
         .csrf().disable()
         .authorizeRequests()
         // 이 입력된 페이지들은 누구나 접근(permitAll())할 수 있도록 합니다.
         .antMatchers("/", "/main", "/memembers/loginerror", "/members/joinform", "/members/join", "/members/welcome").permitAll()
         // hasRole()은 입력된 페이지들을 로그인도 되어있고 "USER"라는 권한을 가지고 있어야 접근할 수 있게 합니다.
         .antMatchers("/securepage", "/members/**").hasRole("USER")
         .anyRequest().authenticated()
         .and()
         	 // 로그인 폼에 대한 설정입니다.
             .formLogin()
             
             // 로그인 폼은 입력된 페이지를 사용합니다. jsp와 컨트롤러가 작성되어 있어야 합니다.
             .loginPage("/members/loginform")
             
             // 로그인 폼에서 input type의 이름을 넣어줍니다. 실제 jsp 파일의 input type 이름과 일치해야 합니다.
             .usernameParameter("userId")
             .passwordParameter("password")
             
             // 폼에서 값을 입력하고 확인을 누르면 해당 경로에서 로그인 처리를 해줍니다.
             // 로그인을 처리하는 Security Filter가 해당 경로를 검사하다가 아이디와 암호가 전달되면 로그인 과정을 처리합니다.
             // 직접 구현하는 것은 아니지만, jsp에서 
             // <form method="post" action="/securityexam/authenticate">처럼 경로를 입력하여 주어야합니다.
             .loginProcessingUrl("/authenticate")
             
             // 로그인 처리가 실패하면  "/loginerror?login_error=1"로 포워딩 됩니다.
             // 해당 jsp와 컨트롤러가 작성되어 있어야 합니다.
             .failureForwardUrl("/members/loginerror?login_error=1")
             
             // 로그인을 성공하면 해당 경로("/")로 리다이렉트 하게 됩니다.
             .defaultSuccessUrl("/",true)
             
             // 로그인 폼은 누구나 접근할 수 있게 합니다.
             .permitAll()
         .and()
         	 // 로그아웃 요청이 오면 세션에서 로그인 정보를 삭제한 후 "/"로 리다이렉트 합니다.
             .logout()
             .logoutUrl("/logout")
             .logoutSuccessUrl("/");
    }

    // 패스워드 인코더를 빈으로 등록합니다. 암호를 인코딩하거나, 
    // 인코딩된 암호와 사용자가 입력한 암호가 같은 지 확인할 때 사용합니다.
    @Bean
    public PasswordEncoder encoder() {
        return new BCryptPasswordEncoder();
    }
}
```

주석으로 설명을 상세하게 달아 놓았기 때문에 따로 설명하지는 않겠습니다.



## 로그인/로그아웃 처리를 위한 클래스 작성하기

아이디와 암호를 전달받아 로그인을 처리하는 것은 `AuthenticationFilter`입니다. `AuthenticationFilter`는 아이디에 해당하는 정보를 읽어 들이기 위해 `UserDetailsService`인터페이스를 구현하는 `빈(Bean)`을 사용합니다.

`UserDetailsService`인터페이스는 스프링 시큐리티에서 제공합니다. 해당 인터페이스를 구현한다는 것은 스프링 시큐리티와 밀접한 연관을 맺는다는 것을 의미합니다. 

그런데, 사용자 정보를 읽어들이는 부분은 스프링 시큐리티와 상관 없을 수도 있습니다.
즉, 스프링 시큐리티와 관련된 부분과 회원 정보를 다루는 부분을 분리하기 위해 다음과 같은 구조로 인터페이스와 클래스를 작성하도록 하겠습니다.

<img src="https://user-images.githubusercontent.com/79291114/119445116-48776f00-bd67-11eb-9551-d09b1d8ca825.jpg" alt="Security-Class-Structure" style="zoom:80%;" />

화질이 안 좋기 때문에 간단히 설명하자면,  

`UserDetailsService`인터페이스를 구현하는 `CustomUserDetailsService(파란색 네모의 오른쪽)`의 `loadUserByUsername()`메소드가 `UserDetails`를 리턴하기 때문에 `UserDetails`를 구현한 `CustomUserDetails(파란색 네모의 왼쪽)`를 필요로 합니다. 또, 데이터베이스에서 로그인 아이디에 해당하는 정보를 읽어 들이기 위해서 `UserDbService(초록색 네모의 왼쪽)`를 구현한 객체를 주입받고 있습니다. 

이 `UserDbService`인터페이스의 `getUser()`메소드는 `UserEntity(초록색 네모의 오른쪽 위)`를 리턴하기 때문에 `UserEntity`를 필요로 하고 `getUserRoles()`의 메소드는 `UserRoleEntity(초록색 네모의 오른쪽 아래)`를 리턴하기 때문에 `UserRoleEntity`를 필요로 합니다.

`UserDbService`인터페이스를 상속받고 있는 `MemberService(분홍색 네모의 위)`와 이 `MemberService`를 구현하고 있는 `MemberServiceImpl(분홍색 네모의 아래)`가 있습니다. `MemberServiceImpl`에서 `UserDbService`의 기능을 구현하고 있습니다.

이렇게 조금은 **복잡하게 구현된 이유는 데이터베이스에서 읽어 들이는 코드와 스프링 시큐리티에서 사용되는 코드를 분리하기 위함**입니다. 아래의 코드를 계속 보게 되면, `UserDbService`에서는 스프링 시큐리티와 관련된 코드가 전혀 사용되고 있지 않은 것을 알 수 있습니다.



### UserEntity.java

`UserDetailsService`을 구현할 때 사용할, 로그인 아이디와 암호 정보를 가지고 있는 `UserEntity`객체를 생성합니다. 

```java
package org.edwith.webbe.securityexam.service.security;

public class UserEntity {
    private String loginUserId;
    private String password;

    public UserEntity(String loginUserId, String password) {
        this.loginUserId = loginUserId;
        this.password = password;
    }

    public String getLoginUserId() {
        return loginUserId;
    }

    public void setLoginUserId(String loginUserId) {
        this.loginUserId = loginUserId;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```



### UserRoleEntity.java

`UserDetailsService`을 구현할 때 사용할, 로그인 아이디와 권한(Role)정보를 가지는 `UserRoleEntity`클래스를 생성합니다.

```java
package org.edwith.webbe.securityexam.service.security;

public class UserRoleEntity {
	private String loginUserId;
    private String roleName;

    public UserRoleEntity(String loginUserId, String roleName) {
        this.loginUserId = loginUserId;
        this.roleName = roleName;
    }

    public String getLoginUserId() {
        return loginUserId;
    }

    public void setLoginUserId(String userLoginId) {
        this.loginUserId = userLoginId;
    }

    public String getRoleName() {
        return roleName;
    }

    public void setRoleName(String roleName) {
        this.roleName = roleName;
    }
}

```



### UserDbService.java

`UserDbService`인터페이스를 작성합니다. 로그인한 사용자 id를 파라미터로 받아서 `UserEntity`와 `List<UserRoleEntity>`를 리턴하는 메소드를 가지고 있습니다. 

로그인 정보가 저장된 데이터베이스의 구조는 프로젝트마다 다르기 때문에 스프링 시큐리티에서 알 수가 없습니다. 즉, 로그인 정보가 어디에 저장되어 있든 해당 인터페이스를 구현하는 쪽에 맡기게 된다는 것을 의미합니다.

```java
package org.edwith.webbe.securityexam.service.security;

import java.util.List;

public interface UserDbService {
    public UserEntity getUser(String loginUserId);
    public List<UserRoleEntity> getUserRoles(String loginUserId);
}
```



### MemberService.java

`UserDbService`인터페이스를 상속받는 `MeberService`인터페이스를 작성합니다. `UserDbService`는 스프링 시큐리티에서 필요로하는 정보를 가지고 오는 메소드를 가지고 있습니다. 

`MemberService`는 회원과 관련된 모든 정보를 처리하는 서비스입니다. 예를 들어 회원 등록과 관련된 메소드는 `MemberService`에 추가되게 됩니다.

```java
package org.edwith.webbe.securityexam.service;

import org.edwith.webbe.securityexam.service.security.UserDbService;

public interface MemberService extends UserDbService {

}
```



### MemberServiceImpl.java

`MemberServiceImpl`클래스는 `MeberService`인터페이스를 구현합니다. `MemberService`인터페이스를 구현한다는 것은 `UserDbService` 역시 구현해야한다는 것을 의미합니다.

임시로 데이터베이스를 읽어들이지 않고 loginUserId가 무엇이든지 간에 `"carami"`라는 사용자 정보를 리턴하게 하겠습니다.

```java
package org.edwith.webbe.securityexam.service;

import java.util.ArrayList;
import java.util.List;

import org.edwith.webbe.securityexam.service.security.UserEntity;
import org.edwith.webbe.securityexam.service.security.UserRoleEntity;
import org.springframework.stereotype.Service;

@Service
public class MemberServiceImpl implements MemberService {
    @Override
    public UserEntity getUser(String loginUserId) {
        return new UserEntity("carami", "$2a$10$G/ADAGLU3vKBd62E6GbrgetQpEKu2ukKgiDR5TWHYwrem0cSv6Z8m");
    }

    @Override
    public List<UserRoleEntity> getUserRoles(String loginUserId) {
        List<UserRoleEntity> list = new ArrayList<>();
        list.add(new UserRoleEntity("carami", "ROLE_USER"));
        return list;
    }
}
```



### CustomUserDetails.java

데이터베이스에서 읽어 들인 로그인 정보는 `UserDetails`인터페이스를 구현하고 있는 객체에 저장되어야 한다고 했습니다. `UserDetails`를 구현하고 있는 `CustomUserDetails`클래스를 생성합니다.

```java
package org.edwith.webbe.securityexam.service.security;

import java.util.Collection;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

public class CustomUserDetails implements UserDetails {
    private String username;
    private String password;
    // 계정의 활성화 여부 (true : 활성화)
    private boolean isEnabled;
    // 계정의 만료 여부 (true : 만료 안됨)
    private boolean isAccountNonExpired;
    // 계정의 잠김 여부 (true : 잠기지 않음)
    private boolean isAccountNonLocked;
    // 계정의 비밀번호 만료 여부 (true : 만료 안됨)
    private boolean isCredentialsNonExpired;
    // 계정의 권한 목록(여러 권한이 있을 수 있음)
    private Collection<? extends GrantedAuthority>authorities ;

    @Override
    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    @Override
    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public boolean isEnabled() {
        return isEnabled;
    }

    public void setEnabled(boolean enabled) {
        isEnabled = enabled;
    }

    @Override
    public boolean isAccountNonExpired() {
        return isAccountNonExpired;
    }

    public void setAccountNonExpired(boolean accountNonExpired) {
        isAccountNonExpired = accountNonExpired;
    }

    @Override
    public boolean isAccountNonLocked() {
        return isAccountNonLocked;
    }

    public void setAccountNonLocked(boolean accountNonLocked) {
        isAccountNonLocked = accountNonLocked;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return isCredentialsNonExpired;
    }

    public void setCredentialsNonExpired(boolean credentialsNonExpired) {
        isCredentialsNonExpired = credentialsNonExpired;
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return authorities;
    }

    public void setAuthorities(Collection<? extends GrantedAuthority> authorities) {
        this.authorities = authorities;
    }
}
```



### CustomUserDetailsService.java

`UserDetailsService`인터페이스를 구현하는 `CustomUserDetailsService`를 생성합니다. `UserDetailsService`인터페이스는 `loadUserByUsername()`1개의 메소드만 선언하고 있습니다.

사용자가 로그인을 할 때 아이디를 입력하면 해당 아이디를 `loadUserByUsername()`메소드의 인자로 전달합니다. 해당 아이디에 해당하는 정보가 없으면 `UsernameNotFoundException`이 발생합니다. 정보가 있을 경우엔 `UserDetails`인터페이스를 구현한 객체를 리턴 하게 됩니다.

```java
package org.edwith.webbe.securityexam.service.security;

import java.util.ArrayList;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service
public class CustomUserDetailsService implements UserDetailsService {

    // CustomUserDbService는 인터페이스입니다. 
	// 해당 인터페이스를 구현하고 있는 객체(MeberServiceImpl)가 Bean으로 등록되어 있어야 합니다.
    @Autowired
    UserDbService userdbService;

    @Override
    // UserDetailsService인터페이스는 loadUserByUsername 1개의 메소드만 선언하고 있습니다.
    // 사용자가 로그인을 할 때 아이디를 입력하면 해당 아이디를 loadUserByUsername()메소드의 인자로 전달합니다. 
    // 해당 아이디에 해당하는 정보가 없으면 UsernameNotFoundException이 발생합니다.
    // 정보가 있을 경우엔 UserDetails인터페이스를 구현한 객체를 리턴 하게 됩니다.
    public UserDetails loadUserByUsername(String loginId) throws UsernameNotFoundException {
    	
        // loginId에 해당하는 정보를 데이터베이스에서 읽어 CustomUser객체에 저장합니다.
        // 해당 정보를 CustomUserDetails객체에 저장합니다.
        UserEntity customUser = userdbService.getUser(loginId);
        if(customUser == null)
            throw new UsernameNotFoundException("사용자가 입력한 아이디에 해당하는 사용자를 찾을 수 없습니다.");
        
        // customUserDetails만들고 userdbService에서 값을 가져와 넣어주고 리턴하면서 파라미터의 아이디와 비교하기 때문에
        // 고정된 값(MemberServiceImpl에서 설정해준 carami)을 나오게 해도
        // 결국 파라미터와 다른 값이기 때문에 Security의 검증 과정에 걸리게 되는 것 같습니다. 
        CustomUserDetails customUserDetails = new CustomUserDetails();
        customUserDetails.setUsername(customUser.getLoginUserId());
        customUserDetails.setPassword(customUser.getPassword());

        List<UserRoleEntity> customRoles = userdbService.getUserRoles(loginId);
  
        // 로그인 한 사용자의 권한 정보를 GrantedAuthority를 구현하고 있는 SimpleGrantedAuthority객체에 담아 리스트에 추가합니다.
        // MemberRole 이름은 "ROLE_"로 시작되어야 합니다.
        List<GrantedAuthority> authorities = new ArrayList<>();
        if(customRoles != null) {
            for (UserRoleEntity customRole : customRoles) {
                authorities.add(new SimpleGrantedAuthority(customRole.getRoleName()));
            } //for
        } //if

        // CustomUserDetails객체에 권한 목록 (authorities)를 설정합니다.
        customUserDetails.setAuthorities(authorities);
        customUserDetails.setEnabled(true);
        customUserDetails.setAccountNonExpired(true);
        customUserDetails.setAccountNonLocked(true);
        customUserDetails.setCredentialsNonExpired(true);
        return customUserDetails;
    }
}
```



## 로그인 처리를 위한 컨트롤러와 뷰 작성하기



### MainController.java

로그인 처리를 위해 로그인 폼을 보여주는 컨트롤러 클래스를 작성합니다.

```java
package org.edwith.webbe.securityexam.controller;

import org.edwith.webbe.securityexam.service.MemberService;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
@RequestMapping(path = "/members")
public class MemberController {
	 // 스프링 컨테이너가 생성자를 통해 자동으로 주입합니다.
    private final MemberService memberService;

    public MemberController(MemberService memberService){
        this.memberService = memberService;
    }

    @GetMapping("/loginform")
    public String loginform(){
        return "members/loginform";
    }

    @RequestMapping("/loginerror")
    public String loginerror(@RequestParam("login_error")String loginError){
        return "members/loginerror";
    }
}
```



### members/loginform.jsp

아이디와 암호를 입력 받는 뷰를 작성합니다. (jsp 파일)

```jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
  <html>
    <head>
      <title>로그인 </title>
    </head>
  <body>
    <div>
      <div>
        <form method="post" action="/securityexam/authenticate">
          <div>
            <label>ID</label>
            <input type="text" name="userId">
          </div>
          <div>
            <label>암호</label>
            <input type="password" name="password">
          </div>
          <div>
            <label></label>
            <input type="submit" value="로그인">
          </div>
        </form>
      </div>
    </div>
  </body>
</html>
```



### members/loginerror.jsp

로그인 오류가 발생할 경우 보여줄 화면을 작성합니다. (jsp 파일)

```jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
  <head>
    <title>로그인 오류</title>
  </head>
  <body>
    <h1>로그인 오류가 발생했습니다. id나 암호를 다시 입력해주세요.</h1>
    <a href="/securityexam/members/loginform">login</a>
  </body>
</html>
```



## 실행



### 로그인

웹 어플리케이션을 실행하고, 다음의 URL을 브라우저에서 실행해봅니다.

`http://localhost:8080/securityexam/members/loginform`

위와 같이 입력하여 로그인 화면이 보여지면, `id 는 carami , 암호는 1234`를 입력하고 로그인 버튼을 클릭합니다. 로그인 처리가 내부적으로 이뤄지고 `main`페이지로 이동하게 됩니다. 

로그인 할 수 있는 id는 `"carami"` 밖에 없습니다. `MemberServiceImpl`에서 입력한 아이디와 상관없이 `"carami"`에 해당하는 정보만 리턴하기 때문입니다.

로그인하지 않으면 들어갈 수 없었던

`http://localhost:8080/securityexam/securepage` 

로 이동해도 잘 보여지는지 확인합니다.



### 로그아웃

`http://localhost:8080/securityexam/logout` 

을 입력하면 로그아웃이 됩니다. 로그아웃을 처리하는 기능을 하나도 구현하지 않았지만 로그아웃을 처리하는 필터가 동작하고 있기 때문에 로그아웃이 됩니다.

로그아웃 이후에는 

`http://localhost:8080/securityexam/securepage`

에 접속하면 403 오류가 발생하면서 로그인 화면으로 넘어가는 것을 알 수 있습니다.

암호가 틀려 로그인을 실패하면 

`http://localhost:8080/securityexam/authenticate` URL 에서 **/members/loginerror페이지가 포워딩되서 보여지는 것을 확인**할 수 있습니다.



---

참고 : [https://www.boostcourse.org/web326/lecture/58999/?isDesc=false](https://www.boostcourse.org/web326/lecture/58999/?isDesc=false)

