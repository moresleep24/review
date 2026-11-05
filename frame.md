# SpringSecurity
## Authentication认证：
！SecurityFilterChain禁用formlogin，session属性，放行login接口  
！login接口中，构造UsernamePasswordAuthenticationToken对象，传入username,password  
！调用authenticationManager.anthenticate(token)方法,trycatch起来，有异常就是登录失败，无异常就登录成功，写入redis，返回uuid  
！实现UserDetailService接口，重写loadUserByUsername方法，该方法需要返回UserDetails实现类，从数据库中查询用户名和密码，构造一个User对象，返回对象。  
！新建一个Filter，Filter继承OncePerRequestFilter，从header中读取Authentication参数，在redis中查询是否存在，存在就设置到SecurityContextHolder中  
！创建好Filter后，配置Filter到SecurityFilterChain中，使用addFilterBefore(tokenAuthenticationFilter, UsernamePasswordAuthenticationFilter.class)
## Authorzation鉴权：
！UserDetailService接口中的loadUserByUsername方法需要返回UserDetails实现类，可以使用User对象  
！User对象有三个参数，分别是username，password，List<SimpleGrantAuthority>，从数据库中读出用户所拥有的权限，配置到list中  
！在需要权限的接口上添加@preAuthorize(hasAuthority(queryUser))，若用户无此权限或权限为空，调用接口即报错  
！每个接口添加注解比较麻烦，可以统一配置，以接口地址作为权限标识，在SecurityFilterChain中的anyRequest().access(this::hasPathAuthority)  
！hasPathAuthority方法中会判断用户权限和当前接口的地址是否匹配，匹配就会返回AuthorzationDecision(true)对象
