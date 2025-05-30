## Interceptor
- 概念：是一种动态拦截方法调用的机制，类似于过滤器。Spring 框架中提供的，主要用来动态拦截控制器方法的执行。
- 作用：拦截请求，在指定的方法调用前后，根据业务需要执行预先设定的代码。
***
## 接口源码
```
public interface HandlerInterceptor {
    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return true;
    }

    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
    }

    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
    }
}
```
- preHandle : 目标资源方法执行前执行，放回 true：放行，返回 false：不放行
- postHandle : 目标资源方法执行后执行
- afterCompletion : 视图渲染完毕后执行，最后执行
***
## 定义拦截器
```
@Slf4j
@Component
public class TokenInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //获取请求路径
        String requestURI = request.getRequestURI();
        //判断是否为登录请求
        if(requestURI.contains("/login")){
            log.info("登录请求,放行");
            return true;
        }
        //获取请求头中的token
        String token = request.getHeader("token");
        //判断是否存在
        if(token == null || token.isEmpty()){
            log.info("令牌为空,响应401");
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
            return false;
        }
        //若存在token，进行校验
        try{
            JwtUtils.parseToken(token);
        }catch (Exception e){
            log.info("令牌非法,响应401");
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
            return false;
        }
        //校验通过，放行
        log.info("令牌合法，放行");
        return true;
    }
}
```
***
## 注册拦截器
```
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private TokenInterceptor tokenInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(tokenInterceptor).addPathPatterns("/**");
    }
}
```