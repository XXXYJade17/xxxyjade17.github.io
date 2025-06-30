# 组件介绍
HttpClient 是 Apache Jakarta Common 子项目，为 Java 提供高效、功能丰富的 HTTP 协议客户端编程工具包，支持 HTTP 协议最新版本与建议 ，方便开发者实现 HTTP 请求发送、响应处理等操作。
# Maven 依赖
```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.13</version>
</dependency>
```
# 使用示例
- 通过 httpclient 发送 GET 请求
```java
/**
 * 测试通过httpclient发送GET请求
 */
@Test
public void testGET() throws IOException {
    //创建httpclient对象
    CloseableHttpClient httpClient = HttpClients.createDefault();
    //创建请求对象
    HttpGet httpGet = new HttpGet("http://localhost:8080/user/shop/status");
    //发送请求,接受响应结果
    CloseableHttpResponse response = httpClient.execute(httpGet);
    //获取服务端返回的状态码
    int statusCode = response.getStatusLine().getStatusCode();
    System.out.println("服务端返回的状态码: "+statusCode);
    //获取服务端返回的数据
    HttpEntity entity = response.getEntity();
    String body = EntityUtils.toString(entity);
    System.out.println("服务端返回的数据为: "+body);
    //关闭资源
    response.close();
    httpClient.close();
}
```
- 通过 httpclient 发送 POST 请求
```java
/**
 * 测试通过httpclient发送POST请求
 */
@Test
public void testPOST() throws IOException {
    //创建httpclient对象
    CloseableHttpClient httpClient = HttpClients.createDefault();
    //创建请求对象
    HttpPost httpPost = new HttpPost("http://localhost:8080/admin/employee/login");
    JSONObject jsonObject = new JSONObject();
    jsonObject.put("username","admin");
    jsonObject.put("password","123456");
    StringEntity entity = new StringEntity(jsonObject.toString());
    //指定请求编码方式,数据格式
    entity.setContentEncoding("utf-8");
    entity.setContentType("application/json");
    httpPost.setEntity(entity);
    //发送请求
    CloseableHttpResponse response = httpClient.execute(httpPost);
    //解析返回结果
    int statusCode = response.getStatusLine().getStatusCode();
    System.out.println("响应码为: "+statusCode);
    HttpEntity entity1 = response.getEntity();
    String body = EntityUtils.toString(entity1);
    System.out.println("响应体为: "+body);
    //关闭资源
    response.close();
    httpClient.close();
}
```