# Annotation



#### @Configuration

* 스프링의 @Configuration 어노테이션은 어노테이션기반 환경구성을 돕는다. 이 어노테이션을 구현함으로써 클래스가 하나 이상의 @Bean 메소드를 제공하고 스프링 컨테이가 Bean정의를 생성하고 런타임시 그 Bean들이 요청들을 처리할 것을 선언하게 된다.

#### @RequiredArgsConstructor

* 이 어노테이션은 초기화 되지않은 final 필드나, @NonNull 이 붙은 필드에 대해 생성자를 생성해 줍니다. 주로 의존성 주입(Dependency Injection) 편의성을 위해서 사용
* 어떠한 빈(Bean)에 생성자가 오직 하나만 있고, 생성자의 파라미터 타입이 빈으로 등록 가능한 존재라면 이 빈은 @Autowired 어노테이션 없이도 의존성 주입이 가능

@EnableScheduling&#x20;

@Scheduled&#x20;



@Component &#x20;

@Service&#x20;

@Controller&#x20;



#### @Component

* 개발자가 직접 컨트롤이 가능한 Class들의 경우

#### @Bean

* 개발자가 컨트롤이 불가능한 외부 라이브러리들을 Bean으로 등록하고 싶은 경우에 사용



#### @Pathvariable

* URL 경로에 변수를 넣어주는 어노테이션

```
@Controller 
public class HomeController {
  @RequestMapping("/student/{studentId}") 
  public String student(@PathVariable String studentId, Model model) {
      model.addAttribute("studentId", studentId); 
      return "student"; 
  } 
}

```

## Cache

@Cacheable&#x20;

@CacheEvict&#x20;

[https://livenow14.tistory.com/56](https://livenow14.tistory.com/56)





## Json

@JsonNaming&#x20;





## WebSocket

#### @EnableWebSocket

* 웹소켓 설정..?

#### @EnableWebSocketMessageBroker

* @Configuration이 선언된 클래스에 추가하면 broker기반의 high-level(STOMP인듯?) WebSocket Messaging을 구현할 수 있다.

#### @MessageMapping

* @MessageMapping은 클라이언트에서 /hello쪽으로 메세지를 전달
* 웹소켓에서 발행하는 경로가 된다. (ex. @MessageMapping("/hello")

#### @SendTo

* 어노테이션에 정의된 쪽으로 결과를 리턴시킨다.
* 웹소켓에서 구독하는 경로가 된다.
* 1:n 으로 메세지를 뿌릴 때 사용하는 구조
* 보통 경로가 /topic (ex. @SendTo("/topic/greetings")

#### @SendToUser

* 1:1으로 메세지를 보낼 때 사용하는 구조
* 웹소켓에서 구독하는 경로가 된다.
* 보통 경로가 /queue
