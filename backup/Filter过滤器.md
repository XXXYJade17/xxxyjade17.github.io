## Filter
  - 概念 : JavaWeb 三大组件（Servlet、Filter、Listener）之一。
  - 过滤器可以把对资源的请求拦截下来，从而实现一些特殊的功能。
  - 过滤器一般完成一些通用的操作，比如：登录校验、统一编码处理、敏感字符处理等。
***
## 接口源码
```
public interface Filter {
    default void init(FilterConfig filterConfig) throws ServletException {
    }

    void doFilter(ServletRequest var1, ServletResponse var2, FilterChain var3) throws IOException, ServletException;

    default void destroy() {
    }
}
```
  - init : 初始化
  - doFilter : 过滤处理
  - destory : 销毁
***
## 配置
  - Filter 类上加 @WebFilter 注解，配置拦截路径
  - 引导类上加 @ServletComponentScan 开启 Servlet 组件支持。

***
## 示例
```
@WebFilter(urlPatterns = "/*")
@Slf4j
public class TokenFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        //参数处理
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;
        //获取请求路径
        String requestURI = request.getRequestURI();
        //判断是否包含"/login"
        if(requestURI.contains("/login")){
            log.info("登录请求，放行");
            filterChain.doFilter(request,response);
            return;
        }
        //获取token
        String token = request.getHeader("token");
        //判断token是否存在
        if(token==null||token.isEmpty()){
            log.info("令牌为空,响应401");
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
            return;
        }
        //存在令牌对其校验
        try{
            JwtUtils.parseToken(token);
        }catch (Exception e){
            log.info("令牌非法,响应401");
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
            return;
        }
        //校验通过放行
        log.info("令牌合法,放行");
        filterChain.doFilter(request,response);
    }
}
```