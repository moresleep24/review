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
# 深分页问题
## 游标分页
！对数据进行主键排序，使用cursor和size参数查询，cursor为上一页最后一条数据的id，size为本次查询的数量  
！查询时使用id>cursor跳过前方数据，再取size条数据  
！问题1 不支持跳页，必须获取到上一页的最后一条数据  
！问题2 需要保证有序，如果排序字段过多，SQL实现复杂  
## 延迟关联
！对数据进行排序后，不直接查询分页的数据，而是查到这批数据的主键，然后用主键去关联当前表，查询所有数据  
！可以减少回表，因为大量的数据并不需要，所以只查询主键。然后关联查询数据  
  SELECT t1.*  
  FROM t_order t1  
  INNER JOIN (  
      -- 这里的子查询可以利用覆盖索引，性能极高  
      SELECT id FROM t_order ORDER BY id LIMIT 1000000, 10  
  ) t2 ON t1.id = t2.id;  
# 支付表结构及退款功能

# 消息队列在系统中有什么应用
