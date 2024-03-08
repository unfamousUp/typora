# SpringSecurity

## SecurityConfig配置类

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    /**
     * 创建BCryptPasswordEncoder注入容器
     * @return
     */
    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    /**
     * 认证处理
     * @return
     * @throws Exception
     */
    @Override
    @Bean
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }

    /**
     * 相关配置
     * @param http
     * @throws Exception
     */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf()
                .disable() // 关闭csrf
                // .ignoringAntMatchers("/doc.html/**") // 允许访问接口文档
                .sessionManagement() // 不通过Session获取SecurityContext
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests() // 放行接口配置
                .antMatchers("/user/login","/doc.html/**")
                .anonymous() // 登录接口可以匿名访问
                .anyRequest()
                .authenticated(); // 除上面以外的所有请求需要鉴权认证
    }

    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers(
                "/doc.html",
                "/v2/api-docs",
                "/swagger-resources/**",
                "/webjars/**");
    }
}
```

## 实现UserDetails接口

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Slf4j
public class LoginUser implements UserDetails {

    private User user;

    /**
     * 返回权限信息
     * @return
     */
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    /**
     * 获取密码
     * @return
     */
    @Override
    public String getPassword() {
        return user.getPassword();
    }

    /**
     * 获取用户名
     * @return
     */
    @Override
    public String getUsername() {
        return user.getUsername();
    }

    /**
     * 判断是否没过期
     * @return
     */
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    // 是否没有超时
    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}

```

## UserDetailsServiceImpl业务层实现类

```java
@Service
@Slf4j
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    private UserMapper userMapper;

    @Override
    /**
     * 查询用户
     */
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 查询用户信息
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.eq("username",username);
        User user = userMapper.selectOne(wrapper); // 查询user_id等信息
        // 查询失败
        if (Objects.isNull(user)){
            throw new RuntimeException("用户名或密码错误");
        }
        //TODO 查询对应的权限信息
        log.info("success");
        // 封装成UserDetails实现类对象
        return new LoginUser(user);
    }
}
```

## 登录验证业务层实现类

```java
@Service
public class LoginServiceImpl implements LoginService {
    @Autowired
    private UserMapper userMapper;

    /**
     * 认证处理
     */
    @Autowired
    AuthenticationManager authenticationManager;

    @Override
    public Result<UserLoginVo> login(UserLoginDTO userLoginDTO) {
        // 1. AuthenticationManager authenticate进行用户认证
        // 认证前封装用户信息到authenticationToken对象中
        UsernamePasswordAuthenticationToken authenticationToken =
                new UsernamePasswordAuthenticationToken(userLoginDTO.getUsername(),userLoginDTO.getPassword());
        // 进行认证：内部调用UserDetailsServiceImpl里的loadUserByUsername方法
        Authentication authentication = authenticationManager.authenticate(authenticationToken);
        // 认证失败：抛出异常
        if(Objects.isNull(authentication)){
            throw new RuntimeException("登录失败");
        }
        // 认证成功：使用查询出的userId生成一个jwt，存储在Result中
        // LoginUser = authentication.getPrincipal();
        // 把用户信息存入redis userid作为key

        Result<UserLoginVo> R = new Result<>();
        // 1.用户密码md5加密
        String password = MD5Util.string2MD5(userLoginDTO.getPassword());
        // 2.根据表单username查询数据库
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.eq("username",userLoginDTO.getUsername());
        User user = userMapper.selectOne(wrapper);
        // 用户名不存在
        if (user == null){
            R.setCode(Code.error);
            return R;
        }
        // 密码错误
        if (!password.equals(user.getPassword())){
            R.setCode(Code.error);
            return R;
        }
        // 登录成功
        R.setCode(Code.success);
        R.setData(new UserLoginVo(
                user.getUserId(),
                user.getUsername(),
                "token"
        ));
        return R;
    }
}
```

