# ๐Simple Login App Using JWT

> ### ๊ฐ๋ฐํ๊ฒฝ
>
> - Frontend : React
> - Backend : Spring Boot 2.3.0
> - JWT, Spring Security
>
> <br>
>
> ### ๊ณต๋ถ ๋ชฉํ
>
> 1. JWT์ ๊ฐ๋๊ณผ ๊ตฌํ ๋ฐฉ๋ฒ
> 2. React๋ก ๊ธฐ๋ณธ์ ์ธ Component ์์ฑ
> 3. Spring Boot๋ก Restful Server ๊ตฌ์ถ
> 4. Frontend์ Backend๋ฅผ ์ฐ๊ฒฐํด JWT์ Token ๊ฐ ์ฒ๋ฆฌ
>
> <br>
>
> ### ์ฐธ๊ณ 
>
> - https://www.udemy.com/course/full-stack-application-with-spring-boot-and-react/
> - https://www.javainuse.com/spring/boot-jwt
> - https://medium.com/swlh/handling-access-and-refresh-tokens-using-axios-interceptors-3970b601a5da

<br>

#### ๋ณด๋ค ์์ธํ ๊ธฐ๋ก์ :notebook_with_decorative_cover:

#### https://velog.io/@dsunni

<br>

# JWT (Json Web Token)

## 1. JWT๋?

- JWT(Json Web Token)๋ ์นํ์ค ([RFC 7519](https://tools.ietf.org/html/rfc7519))์ผ๋ก JSON ํฌ๋งท์ ์ด์ฉํด ์ ๋ณด๋ฅผ ๊ฐ๋ณ๊ณ  ์์ ํ๊ฒ ์ ์กํ๊ธฐ ์ํ **Claim ๊ธฐ๋ฐ์ Web Token**์ด๋ค. 
- ์๋ฒ๋ง ์๊ณ  ์๋ Secret Key๋ก ๋์งํธ ์๋ชํ๋์ด์๊ธฐ ๋๋ฌธ์ ์ ๋ขฐํ  ์ ์๋ค
- ๋ณดํต **Authorization** (๋ก๊ทธ์ธ, SSO) ๋๋ **์์ ํ ์ ๋ณด ๊ตํ**์ ์ํด ์ฌ์ฉ๋๋ค.
- JWT์์๋ ํ ํฐ ์์ฒด์ ์ ์  ์ ๋ณด๋ฅผ ๋ด์์ HTTP ํค๋์ ์ ๋ฌํ๊ธฐ์ *์ ์  ์ธ์์ ์ ์งํ  ํ์๊ฐ ์๋ค*.

<br>

### ํด๋ ์(Claim)
  - ํด๋ ์(Claim)์ด๋ ์ฌ์ฉ์ ์ ๋ณด๋ ๋ฐ์ดํฐ ์์ฑ ๋ฑ์ ์๋ฏธํ๋ค.
  - ํด๋ ์ ๊ธฐ๋ฐ ํ ํฐ ์์๋ ์ฌ์ฉ์์ id, pw ๋ฑ์ ๊ฐ์ธ ์ ๋ณด๊ฐ ๋ค์ด์๋ค.
      - `Self-contained` : ์์ฒด ํฌํจ, ํ ํฐ ์์ฒด๊ฐ ์ ๋ณด
  - JWT๋ ๊ฐ์ฅ ๋ํ์ ์ธ ํด๋ ์ ๊ธฐ๋ฐ ํ ํฐ์ด๋ค.

<br>

### JWT์ ํ์์ฑ

- **Session**์ ํ๊ณ
  - Cookie๋ ์ ๋ณด๋ฅผ ํด๋ผ์ด์ธํธ ์ธก์ ์ ์ฅํ๊ณ  Session์ ์ ๋ณด๋ฅผ ์๋ฒ์ธก์ ์ ์ฅํ๋ค.
  - ๋ฐ๋ผ์ ์ ์ ์ ์๊ฐ ๋๋ฌด ๋ง์ผ๋ฉด ์๋ฒ ๊ณผ๋ถํ

- **Scale Out**์ ํ๊ณ
  - ์๋ฒ ํ์ฅ(scale out)์ ์ธ์ ์ ๋ณด ๋๊ธฐํ ๋ฌธ์ 
- REST API๋ **Stateless**๋ฅผ ์งํฅ
  - ์ฌ์ฉ์์ ์ํ ์ ๋ณด๋ฅผ ์ ์ฅํ์ง ์๋ ํํ ex) ์ธ์, ์ฟ ํค

<br>

<br>

## 2.  JWT ํ์

### header.payload.signature
<br>

![Structure of JWT](https://www.javainuse.com/63_6-min.JPG)

- Encoded
  - Base64๋ก ์ธ์ฝ๋ฉ ๋์ด .์ ๊ตฌ๋ถ์๋ก 3๊ฐ์ง์ ๋ฌธ์์ด๋ก ๊ตฌ์ฑ๋์ด์๋ค.
- Decoded
  1. Header : type, algorithm
  2. Payload : not mandatory / Additional information
  3. Signature
     - Base64 Encoded header + payload
     - **512 bit secret key** (base64 Encoded)

<br>

<br>

## 3. ๋์ ๊ณผ์ 

![Spring Boot JWT Workflow](https://www.javainuse.com/62-12-min.JPG)

1. ํด๋ผ์ด์ธํธ ๋ก๊ทธ์ธ ์์ฒญ POST(id, pw)
2. ์๋ฒ๋ (id, pw)๊ฐ ๋ง๋์ง ํ์ธ ํ ๋ง๋ค๋ฉด **JWT**๋ฅผ Secret Key๋ก **์์ฑ** ํ ์ ๋ฌ
3. ํด๋ผ์ด์ธํธ๋ Token์ **Local Storage** / Cookie์ **์ ์ฅ**
4. ํด๋ผ์ด์ธํธ๋ ์๋ฒ์ ์์ฒญํ  ๋ ํญ์ **ํค๋์ Token์ ํฌํจ**์ํด
   - axios์ Interceptor ์ฌ์ฉ
5. ์๋ฒ๋ ์์ฒญ์ ๋ฐ์ ๋๋ง๋ค Secret Key๋ฅผ ์ฌ์ฉํด**Token์ด ์ ํจํ์ง ๊ฒ์ฆ**
   - ์๋ฒ๋ง์ด Secret Key๋ฅผ ๊ฐ์ง๊ณ  ์๊ธฐ ๋๋ฌธ์ ๊ฒ์ฆ ๊ฐ๋ฅ
   - Token์ด ๊ฒ์ฆ๋๋ฉด ๋ฐ๋ก username, pw๋ฅผ ๊ฒ์ฌํ์ง ์์๋ user identification ๊ฐ๋ฅ
6. ์๋ฒ์ **Response**

<br>

## 4. ๊ฒ์ฆ ๊ณผ์ 

1. ํด๋ผ์ด์ธํธ์ ์์ฒญ (Header : Token)
2. Spring์ Interceptor์ ์ํด ์์ฒญ์ด Intercept๋จ
3. ํด๋ผ์ด์ธํธ์๊ฒ ์ ๊ณต๋์๋ Token๊ณผ ํด๋ผ์ด์ธํธ์ Header์ ๋ด๊ธด Token ์ผ์น ํ์ธ
4. `auth0 JWT`๋ฅผ ์ด์ฉํด `issuer, expire` ๊ฒ์ฆ

<br>

<br>

# Simple Login App์ ํ๋ฆ

> - Making use of **hard coded user values** for User Authentication
>   - id : user_id
>   - password : user_pw

<br>

## 1. JWT ์์ฑ

- **POST** API with mapping **/authenticate**
- id: user_id, pw: user_pw ์๋ ฅ์ ์ฌ์ฉ์ ํ์ธ ํ JWT ์์ฑ

<br>

![Spring Boot JWT Generate Token](https://www.javainuse.com/62-2-min.JPG)

<br>

<br>

## 2. JWT ๊ฒ์ฆ

- GET API with mapping /hello
- user๊ฐ ์ ํจํ JWT๋ฅผ ๊ฐ์ง๊ณ  ์์ ๋ ์ ๊ทผ์ด ํ์ฉ๋จ

<br>

![Spring Boot JWT Validate Token](https://www.javainuse.com/62-3-min.JPG)

