# Overview
At first, it is a login portal with provided credentials.

<img width="1920" height="531" alt="Screenshot_2025-09-23_22-44-19" src="https://github.com/user-attachments/assets/f17704db-27c3-4017-9d4f-9cf183eac2c2" />

After enter the provided credentials, I got in under `user` permission. The interface turns into an online book library with 3 books, each has a different permission such as `free`, `premium`, and `admin`.

<img width="1920" height="933" alt="Screenshot_2025-09-23_22-51-16" src="https://github.com/user-attachments/assets/b7f71d84-259e-4f0d-bbe8-c6dbf62a7582" />

# Actions
Now I need to intercept the request to look for something suspcious, at first glance I assume that this site has a vulnerability related to jwt because I looked at the source code of the site which kinda gave me a hint. However, I still need to intercept the request.

In the file `src/main/java/io/github/nandandesai/pico/security/SecretGenerator.java`, I learnt that there is a hidden file called `server_secret.txt` in the code below:
```
@Service
class SecretGenerator {
    private Logger logger = LoggerFactory.getLogger(SecretGenerator.class);
    private static final String SERVER_SECRET_FILENAME = "server_secret.txt";
```
<img width="1689" height="206" alt="Screenshot_2025-09-23_23-18-46" src="https://github.com/user-attachments/assets/82d7660f-d281-4257-be32-e419e88921bc" />

So I might assume that if I intecept this request what could I get?
<img width="1226" height="671" alt="Screenshot_2025-09-23_23-33-51" src="https://github.com/user-attachments/assets/6c8b3cce-10aa-4222-bd01-c1c9945730af" />
I mean atleast I got a token, now I need to crack it to modify the permission from `user` to `admin` in order to get the flag

I am kinda confused at that momemnt so I might have a look at the README.md to learn more about the architecture of this site and notice these interesting folders
<img width="945" height="184" alt="Screenshot_2025-09-24_08-46-19" src="https://github.com/user-attachments/assets/4411679c-ed0c-482f-963f-e09457387036" />

It said that `/security` folder contains JWT generator function which is what I need right now

Inside JWT services, I understand how they use HMAC256 to decdoe the token so, the next thing that I am going to do is using `cyberchef` and decode the `user` token
```
 public JwtUserInfo decodeToken(String token) throws JWTVerificationException {
        Algorithm algorithm = Algorithm.HMAC256(SECRET_KEY);
        JWTVerifier verifier = JWT.require(algorithm)
                .withIssuer(ISSUER)
                .build();
        DecodedJWT jwt = verifier.verify(token);
        Integer userId = jwt.getClaim(CLAIM_KEY_USER_ID).asInt();
        String email = jwt.getClaim(CLAIM_KEY_EMAIL).asString();
        String role = jwt.getClaim(CLAIM_KEY_ROLE).asString();
        return new JwtUserInfo().setEmail(email)
                .setRole(role)
                .setUserId(userId);
    }
```
For whom do not know where did I get the token? Intercepting the request at the login page
<img width="1226" height="462" alt="Screenshot_2025-09-24_08-59-16" src="https://github.com/user-attachments/assets/d7134d63-4ced-424b-a2f1-d29a55198426" />

But when I inputed this token, the jwt decoder site response with invalid signature and the length of the string is not enough then I realize that the token should also have the keyword `Bearer` as well.
One more thing I need to illustrate is that I could not decode it with `cyberchef` because it return random string and I did not know they secret signing key either. I could not find anything in the `sever_secret.txt`. However, after browsing back, to that file
I can just assume that the random string is `1234` base on the source code below:

```
@Service
class SecretGenerator {
    private Logger logger = LoggerFactory.getLogger(SecretGenerator.class);
    private static final String SERVER_SECRET_FILENAME = "server_secret.txt";

    @Autowired
    private UserDataPaths userDataPaths;

    private String generateRandomString(int len) {
        // not so random
        return "1234";
    }
```
Now I am going to attempt to get the jwt token for decoding 
<img width="1550" height="393" alt="Screenshot_2025-09-24_09-48-49" src="https://github.com/user-attachments/assets/90939ed2-9909-4598-a9c4-cd2e6653e615" />


Using [jwt.io](https://www.jwt.io/) to decode 
<img width="1313" height="755" alt="Screenshot_2025-09-24_09-49-17" src="https://github.com/user-attachments/assets/ed62c82f-626c-4e2c-bfa4-893e4141ea2f" />



What should I do next? How can I modify the payload? Let's have a look at the `Role.java` file
```
public class Role {
    @Id
    @Column
    private String name;

    @Column
    private Integer value; //higher the value, more the privilege. By this logic, admin is supposed to
    // have the highest value
}
```
Jump to JWT Encoder, get the value of userId increment by 1, role to `Admin`, email to `admin` as well but remember the character sensitive
<img width="1288" height="647" alt="Screenshot_2025-09-24_09-49-43" src="https://github.com/user-attachments/assets/c4f5dad1-5d35-46a3-a20e-693bc08f0c8d" />

Then accessing `/pdf/5` with the payload to get the flag
<img width="1544" height="820" alt="Screenshot_2025-09-24_09-50-41" src="https://github.com/user-attachments/assets/b72db59f-450a-42e1-80f5-b4d4fb4b8f62" />

Happy Hacking!!!!!~`


```
picoCTF{w34k_jwt_n0t_g00d_42f5774a}
```
