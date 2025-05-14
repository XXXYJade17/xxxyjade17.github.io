## 如何定义一个全局异常处理类
```
@Slf4j
@RestControllerAdvice
public class GlobalExceptionHandler {
}
```
  **@RestControllerAdvice  = @ControllerAdvice + @ResponseBody**
## 捕获异常
```
@ExceptionHandler
public Result handleException(Exception e){
}
```
