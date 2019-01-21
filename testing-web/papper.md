```java
package hello;

import org.junit.Test;
import org.junit.runner.RunWith;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.context.SpringBootTest.WebEnvironment;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.boot.web.server.LocalServerPort;
import org.springframework.test.context.junit4.SpringRunner;

import static org.assertj.core.api.Assertions.assertThat;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
public class HttpRequestTest {

    @LocalServerPort
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    public void greetingShouldReturnDefaultMessage() throws Exception {
        assertThat(this.restTemplate.getForObject("http://localhost:" + port + "/",
                String.class)).contains("Hello World");
    }
}
```
* @SpringBootTest可以去寻找那个被注解为@SpringBootApplication的类，然后加载那些应该加载的配置，来形成一个Spring context（上下文）；
* 这种方式是启动容器的方式，其中webEnvironment = WebEnvironment.RANDOM_PORT参数可以给容器随机指定一个端口，免得和现有启动的容器造成端口冲突；
* TestRestTemplate对象是在注解SpringBootTest标签的时候就准备好的，我们只需要Autowired进行注入和使用就行了，该对象可以模拟我们的HTTP请求，判断返回的结果
