使用Ribbon实现客户端负载均衡的消费者
1) pom.xml增加Ribbon(spring-cloud-starter-ribbon)、eureka(spring-cloud-starter-eureka)配置
2) 在应用主类Application增加@EnableDiscoveryClient注解来添加发现服务能力
3) 创建RestTemplate实例，并通过@LoadBalanced注解开启均衡负载能力。
	@Bean//@Bean标注在方法上(返回某个实例的方法)，等价于spring的xml配置文件中的<bean>，作用为：注册bean对象
	@LoadBalanced
	RestTemplate restTemplate() {
		return new RestTemplate();
	}
4) 创建ConsumerController来消费COMPUTE-SERVICE的add服务。通过直接RestTemplate来调用服务，计算10 + 20的值
@RestController
public class ConsumerController {
    @Autowired
    RestTemplate restTemplate;
    @RequestMapping(value = "/add", method = RequestMethod.GET)
    public String add() {
        return restTemplate.getForEntity("http://COMPUTE-SERVICE/add?a=10&b=20", String.class).getBody();
    }
}
5) application.properties中配置eureka服务注册中心
spring.application.name=ribbon-consumer
server.port=3333
eureka.client.serviceUrl.defaultZone=http://localhost:1111/eureka/



Ribbon请求服务地址：
http://10.5.2.241:3333/add
