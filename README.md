# Instagram Application
This Application Uses MySQL Database...
##### :red_square: Its an API Based Web Application
## 🟩: Frameworks and Languages Used -
    1. SpringBoot
    2. JAVA
    3. Postman
    4. SQL
    
## 🟩: Dependency Used
    1. Spring Web
    2. Spring Boot Dev Tools
    3. Lombok
    4. Spring Data JPA
    5. MySQL Driver
    6. Jakarta
    7. Javax
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 🟩: Dataflow (Functions Used In)
### :red_square: 1. Model - Model is used to Iniitialize the required attributes and create the accessable constructors and methods
#### ➡️: User.java
```java
@Entity
@Table(name= "tabl_User")
public class User {
    @Id
    @Column(name="userId")
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Integer userId;
    private String userFirstName;
    private String userLastName;
    private Integer userAge;
    private String userEmail;
    private String userPassword;
    private String userPhoneNumber;
}
```
#### ➡️: Post.java
```java
@Entity
@Table(name="table_Post")
public class Post {
    @Id
    @Column(name="PostId")
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Integer postId;
    private Timestamp createDate;
    private Timestamp updateDate;
    private String postData;

    @JoinColumn(name="UserId")
    @ManyToOne(fetch = FetchType.LAZY)
    private User user;

}
```
#### ➡️: AuthenticationToken.java
```java
public class AuthenticationToken {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long tokenId;

    private String token;
    private LocalDate tokenCreationDate;

    @OneToOne
    @JoinColumn(nullable = false, name = "fk_user_Id")
    private User user;

    public AuthenticationToken(User user) {
        this.user = user;
        this.tokenCreationDate = LocalDate.now();
        this.token = UUID.randomUUID().toString();
    }
}
```
##### To See Model
:white_check_mark: [Instagram-Model](https://github.com/tejasviii/Instagram/tree/master/src/main/java/com/example/Instagram/Model)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

### :red_square: 2. Service - This Layer is used to write the logic of our CRUD operaions.
#### ➡️: UserService.java
```java
@Service
public class UserService {

    @Autowired
    IUserRepository userRepository;
    
    @Autowired
    AuthenticationService authService;
    
    public SignInOutput signIn(SignInInput signInDto) {
        //check Email
        ------
        //encrypt Password
        ------

        //match it with database encrypted password
        boolean isPasswordValid = encryptedPassword.equals(user.getUserPassword());
        if(!isPasswordValid){
            throw new IllegalStateException("User Invalid!!....Signup Again!!");
        }

        //figure out the token
        -------
        //setup output response
        -------
    }
}
```

#### ➡️: ProductService.java
```java
@Service
public class PostService {

    @Autowired
    IPostRepository postRepository;
    public int savePost(Post post) {
        Post savedPost = postRepository.save(post);
        return savedPost.getPostId();
    }
}
```

#### ➡️: AuthenticationService.java
```java
@Service
public class AuthenticationService {

    @Autowired
    ITokenRepo tokenRepo;

    public void saveToken(AuthenticationToken token){
        tokenRepo.save(token);
    }

    public AuthenticationToken getToken(User user) {
        return tokenRepo.findByUser(user);
    }
}
```

#### To See Service
:white_check_mark: [Instagram-Service](https://github.com/tejasviii/Instagram/tree/master/src/main/java/com/example/Instagram/Service)
----------------------------------------------------------------------------------------------------------------------------------------------------

### :red_square: 3. Controller - This Controller is used to like UI between Model and Service and also for CRUD Mappings.
#### ➡️: UserController.java
```java
@RestController
@RequestMapping("api/v1/userApp")
public class UserController {

    @Autowired
    UserService userService;

    //SignUp
    @PostMapping("/signup")
    public SignUpOutput signUp (@RequestBody SignUpInput signUpDto){
        return userService.signUp(signUpDto);
    }
}
```

#### ➡️: PostController.java
```java
@RestController
public class PostController {

    @Autowired
    IUserRepository userRepository;

    @Autowired
    PostService postService;

    @PostMapping(value="/post")
    public ResponseEntity<String> savePost(@RequestBody String postRequest){
        Post post = setPost(postRequest);
        int postId = postService.savePost(post);
        return new ResponseEntity<>("post saved with Id - " + postId, HttpStatus.CREATED);
    }
}
```

#### To See Controller
:white_check_mark: [Instagram-Controller](https://github.com/tejasviii/Instagram/tree/master/src/main/java/com/example/Instagram/Controller)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
### :red_square: 3. Repository : data access object (DAO) is an object that provides an abstract interface to some type of database or other persistence mechanisms.
#### ➡️: IUserRepository.java
```java
@Repository
public interface IUserRepository extends JpaRepository<User,Integer> {
    User findFirstByUserEmail(String email);
}
```

#### ➡️: IPostRepository.java
```java
@Repository
public interface IPostRepository extends JpaRepository<Post,Integer> {
}
```

#### ➡️: ITokenRepo.java
```java
@Repository
public interface ITokenRepo extends JpaRepository<AuthenticationToken,Long> {
    AuthenticationToken findByUser(User user);
}
```

#### To See Repository
:white_check_mark: [Instagram-DAO](https://github.com/tejasviii/Instagram/tree/master/src/main/java/com/example/Instagram/Repository)
-------------------------------------------------------------------------------------------------------------------------------------------------------
### :red_square: 4. DTO :  This Layer is for SignIn and SignUp Authentication
#### ➡️: SignUpInput.java

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class SignUpInput {
    private String userFirstName;
    private String userLastName;
    private Integer userAge;
    private String userEmail;
    private String userPassword;
    private String userContact;
}
```
#### ➡️: SignUpOutput.java

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class SignUpOutput {

    String status;
    String message;
}
```

#### ➡️: SignInInput.java

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class SignInInput {

    private String userEmail;
    private String userPassword;
}
```

#### ➡️: SignInOutput.java
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class SignInOutput {

    private String status;
    private String message;
}
```

#### To See DTO
:white_check_mark: [Instagram-DTO](https://github.com/tejasviii/Instagram/tree/master/src/main/java/com/example/Instagram/Dto)    
-------------------------------------------------------------------------------------------------------------------------------------------------------
## 🟩: DataStructures Used in Project
    1. ResponseEntity
    2. List
    3. Json
-------------------------------------------------------------------------------------------------------------------------------------------------------
## 🟩: DataBase Response In project

:arrow_right: User table
 ```sql
 select * from tabl_user;
+---------+----------+----------------------+------------+------------+------------+----------------------------------+
| user_id | user_age | user_email           | first_name | last_name  | user_phone | user_password                    |
+---------+----------+----------------------+------------+------------+------------+----------------------------------+
|       1 |       15 | Dimpi15@gmail.com    | Dimpi      | Chauhan    | 5146513462 | Dimpi@1234566                    |
|       2 |       23 | tejasvicha@gmail.com | Tejasvi    | Chauhan    | 8426074533 | tccw$##@123                      |
|       3 |       50 | sandy@gmail.com      | Sandeep    | Chauhan    | 2165432164 | NULL                             |
+---------+----------+----------------------+------------+------------+------------+----------------------------------+
```
:arrow_right: Post Table 
```sql
 select * from product;
+------------+-----------------+----------+-------------------------------+-------+---------------------+
| product_id | brand           | category | description                   | price | product_name        |
+------------+-----------------+----------+-------------------------------+-------+---------------------+
|          1 | Blackberrys     | Gents    | Silky Shirts && cotton mix    |  3999 | Silky Shirt         |
|          2 | Red Tape        | Gents    | Kurta                         |   1999| Kurta               |
|          3 | Peter England   | Gents    | pants                         |  1500 | pants               |
|          4 | Zodiac          | Gents    | pajama                        |   999 | pajama              |
|          5 | Levi's          | Gents    | Levi's T-shirt                |   599 | Levi's T-shirt      |
+------------+-----------------+----------+-------------------------------+-------+---------------------+
```

:arrow_right: Address Table 

```sql
 select * from table_post;
+---------+-------------------------+-----------+-------------+---------+--------------------------------------------------+
| post_id | created_date            | post_date | update_date | user_id | post_data                                        |
+---------+-------------------------+-----------+-------------+---------+--------------------------------------------------+
|       6 | 2022-03-10 13:17:16.725 | NULL      | NULL        |       1 | data which is hiding by admin, essential data.   |
|       7 | 2010-03-10 13:18:29.099 | NULL      | NULL        |       1 | data which is hiding by admin, essential data.   |
|       8 | 2012-03-10 13:32:40.327 | NULL      | NULL        |       2 | Life is beautiful with positive people.          |
|       3 | 2007-05-10 12:45:41.418 | NULL      | NULL        |       1 | I am Best in studies..!!!                        |
+---------+-------------------------+-----------+-------------+---------+--------------------------------------------------+
```
:arrow_right: AuthenticationToken Table 

```sql
 select * from authentication_token;
+----------+--------------------------------------+---------------------+------------+
| token_id | token                                | token_creation_date | fk_user_id |
+----------+--------------------------------------+---------------------+------------+
|        1 | 98e04d49-6aff-46f5-8020-6fe24a652ec9 | 2017-02-20          |         5  |
+----------+--------------------------------------+---------------------+------------+
```
----------------------------------------------------------------------------------------------------------------------------------------------------------
