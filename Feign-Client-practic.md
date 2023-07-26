Цель: создание API для библиотеки.

Требования:

 

Создай новый сервис с названием BookService.

Создай в нем модель Book.

@Data
@Builder
@AllArgsConstructor
@Document(collection = "books")
public class Book {
   @Id
   private String id;
   private String title;
   private String description;
   private String imageLink;
}
Подключи к сервису MongoDb.

Создай слой service, а также слой Spring Data для работы с сущностью Book и Mongo, потребуется лишь метод findAll.
У тебя должно получиться четыре приложения: Eureka Server, Config Server, Client-Service, Book-Service. Заготовки ты можешь найти в репозитории: https://github.com/KataAcademy/srping-cloud-course.
Создай RestController с маппингом “/api/books”, который будет иметь один метод public List<Book> getAllBooks — отдает все книги с базы, используя сервис и репозиторий Spring Data.
Сделай этот сервис клиентом эврики, имя сервиса — book-service.
Модифицируй существующий client-service: добавь в него зависимость для Feign Client и не забудь проставить аннотацию @EnableFeignClients. 
Добавь в сервис модель Book, такую же, как и в book-service.
Создай Feign Client с одним методом GET, который будет обращаться на RestController book сервиса. Получаем все книги, что есть в базе у book-service.
Создай RestController с маппингом /api/client и один метод List<Book> getAllBooksFromClient с маппингом /books — метод использует созданный ранее FeignClient, чтобы отдать книги.
Протестируй приложение, послав GET запрос на /api/client/books.

Данил Белоусов
30.04.2023 11:30
16 часов ушло на эту задачу
создать образ докера можно создав файл docker-compose.yaml
version: '3'
services:
  mongodb:
    image: mongo
    ports:
      - "27018:27017"
создать экземпляры класса book можно создав следующий класс
@Configuration
public class DatabaseConfig {

    private final MongoTemplate mongoTemplate;

    @Autowired
    public DatabaseConfig(MongoTemplate mongoTemplate) {
        this.mongoTemplate = mongoTemplate;
    }

    @Bean
    public CommandLineRunner commandLineRunner() {
        return args -> {
            // Создаем список книг для сохранения в базу данных
            List<Book> books = Arrays.asList(
                    Book.builder().title("Book 1").description("Description 1").imageLink("Link 1").build(),
                    Book.builder().title("Book 2").description("Description 2").imageLink("Link 2").build(),
                    Book.builder().title("Book 3").description("Description 3").imageLink("Link 3").build()
            );
            // Сохраняем книги в базу данных
            mongoTemplate.insertAll(books);
        };
    }
}
то что mongotemplate горит красным так и должно быть
