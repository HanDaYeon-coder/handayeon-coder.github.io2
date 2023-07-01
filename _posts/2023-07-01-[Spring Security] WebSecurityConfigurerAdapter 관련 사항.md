---
title: "[Spring Security] WebSecurityConfigurerAdapter 관련 사항"
date: 2023-07-01
---

### PART 1. WebSecurityConfigurerAdapter 관련 에러 발생

이전 책과 자료를 참고하여 Spring Security 관련 설정 파일을 작성하다보면 `'org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter' is deprecated ` 라는 설정 파일을 확인할 수 있었습니다.

![](https://velog.velcdn.com/images/da_na/post/d44557ac-f174-4e1b-b077-97ee449eb556/image.png)

따라서 해당 에러 메시지에 따르면, `WebSecurityConfigurerAdapter`를 사용하지 않아야 합니다.

아래의 코드는 경고가 발생한 코드입니다.

경고 메시지는 나왔지만, 해당 코드를 사용해도 에러가 발생하지 않고 정상적으로 작동하는 모습을 확인할 수 있었습니다.


```java
package com.forever.dadamda.config;

import com.forever.dadamda.entity.Role;
import com.forever.dadamda.service.CustomOAuth2UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@RequiredArgsConstructor
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final CustomOAuth2UserService customOAuth2UserService;

    @Override
    protected void configure(HttpSecurity http) throws Exception {

        http
                .csrf().disable()
                .headers().frameOptions().disable()
                .and()
                    .authorizeRequests()
                    .antMatchers("/", "/css/**", "/images/**",
                        "/js/**","/h2-console/**").permitAll()
                    .antMatchers("/api/v1/**").hasRole(Role.USER.name())
                    .anyRequest().authenticated()
                .and()
                    .logout()
                    .logoutSuccessUrl("/")
                .and()
                    .oauth2Login()
                        .userInfoEndpoint()
                            .userService(customOAuth2UserService);
    }
}
```
![](https://velog.velcdn.com/images/da_na/post/4fc49b2d-bafe-44ab-b0ff-0a52afdfeed5/image.png)

☝️ 그러나, 최대한 Spring Security에서 권장하는 코드로 변경하는 것이 나중에 코드 유지보수를 위해서라도 좋다고 판단하여 아래의 공식 문서를 참고하여 코드를 변경하였습니다.

https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter
![](https://velog.velcdn.com/images/da_na/post/f27afcf1-3785-4d5f-842b-9c096a3325d9/image.png)

```java
package com.forever.dadamda.config;

import com.forever.dadamda.entity.Role;
import com.forever.dadamda.service.CustomOAuth2UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Bean;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;

@RequiredArgsConstructor
@EnableWebSecurity
public class SecurityConfig {

    private final CustomOAuth2UserService customOAuth2UserService;

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

         http
                .csrf().disable()
                .headers().frameOptions().disable()
                .and()
                .authorizeRequests()
                .antMatchers("/", "/css/**", "/images/**",
                        "/js/**","/h2-console/**").permitAll()
                .antMatchers("/api/v1/**").hasRole(Role.USER.name())
                .anyRequest().authenticated()
                .and()
                .logout()
                .logoutSuccessUrl("/")
                .and()
                .oauth2Login()
                .userInfoEndpoint()
                .userService(customOAuth2UserService);

         return http.build();
    }
}
```
