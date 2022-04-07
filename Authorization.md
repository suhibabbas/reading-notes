# Spring Authorization

1. **Authorization Server**
is a supreme architectural component for Web API Security. The Authorization Server acts a centralization authorization point that allows your apps and HTTP endpoints to identify the features of your application.

2. **Resource Server**
is an application that provides the access token to the clients to access the Resource Server HTTP Endpoints. It is collection of libraries which contains the HTTP Endpoints, static resources, and Dynamic web pages.

3. **OAuth2**
is an authorization framework that enables the application Web Security to access the resources from the client. To build an OAuth2 application.

4. **JWT Token**
is a JSON Web Token, used to represent the claims secured between two parties.

There are several samples building on each other, adding new features at each step:

- **simple:** a very basic static app with just a home page and unconditional login via Spring Boot’s OAuth 2.0 configuration properties (if you visit the home page, you will be automatically redirected to GitHub).

- **click:** adds an explicit link that the user has to click to login.

- **logout:** adds a logout link as well for authenticated users.

- **two-providers:** adds a second login provider so the user can choose on the home page which one to use.

- **custom-error:** adds an error message for unauthenticated users, and a custom authentication based on GitHub’s API.

## Single Sign On With GitHub

well create a minimal application that uses GitHub for authentication.

### Add a Home Page

create `index.html` in the `src/main/resources/static` folder, and add some stylesheets and javaScript

**Ex:**

```html
    <!doctype html>
    <html lang="en">
    <head>
        <meta charset="utf-8"/>
        <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
        <title>Demo</title>
        <meta name="description" content=""/>
        <meta name="viewport" content="width=device-width"/>
        <base href="/"/>
        <link rel="stylesheet" type="text/css" href="/webjars/bootstrap/css/bootstrap.min.css"/>
        <script type="text/javascript" src="/webjars/jquery/jquery.min.js"></script>
        <script type="text/javascript" src="/webjars/bootstrap/js/bootstrap.min.js"></script>
    </head>
    <body>
        <h1>Demo</h1>
        <div class="container"></div>
    </body>
    </html>
```

### Securing the Application with GitHub and Spring Security

add Spring Security as a dependency,should include the Spring Security OAuth 2.0 Client starter:


### Add a New GitHub App

Select "New OAuth App" and then the "Register a new OAuth application" page is presented. Enter an app name and description.

The default redirect URI template is {baseUrl}/login/oauth2/code/{registrationId}. The registrationId is a unique identifier for the ClientRegistration.

Then, to make the link to GitHub, add the following to your `application.yml`:

```yml
spring:
  security:
    oauth2:
      client:
        registration:
          github:
            clientId: github-client-id
            clientSecret: github-client-secret
```

### Add a Welcome Page

**Conditional Content on the Home Page**
To render content on the condition that the user is authenticated, you have the option of either server-side or client-side rendering.

To get started with the dynamic content, you need to mark a couple of HTML elements like so:

```html
<div class="container unauthenticated">
    With GitHub: <a href="/oauth2/authorization/github">click here</a>
</div>
<div class="container authenticated" style="display:none">
    Logged in as: <span id="user"></span>
</div>
```

**The user Endpoint**
well add the server-side endpoint just mentioned, calling it `/user`.

```java
@SpringBootApplication
@RestController
public class SocialApplication {

    @GetMapping("/user")
    public Map<String, Object> user(@AuthenticationPrincipal OAuth2User principal) {
        return Collections.singletonMap("name", principal.getAttribute("name"));
    }

    public static void main(String[] args) {
        SpringApplication.run(SocialApplication.class, args);
    }

}
```

**Making the Home Page Public**

```java
@SpringBootApplication
@RestController
public class SocialApplication extends WebSecurityConfigurerAdapter {

    // ...

    @Override
    protected void configure(HttpSecurity http) throws Exception {
    	// @formatter:off
        http
            .authorizeRequests(a -> a
                .antMatchers("/", "/error", "/webjars/**").permitAll()
                .anyRequest().authenticated()
            )
            .exceptionHandling(e -> e
                .authenticationEntryPoint(new HttpStatusEntryPoint(HttpStatus.UNAUTHORIZED))
            )
            .oauth2Login();
        // @formatter:on
    }

}
```

### Add a Logout Button

it's not a simple feature, it requires a bit of care to implement.

#### Client Side Changes

need to provide a logout button and some javaScript

```html
<div class="container authenticated">
  Logged in as: <span id="user"></span>
  <div>
    <button onClick="logout()" class="btn btn-primary">Logout</button>
  </div>
</div>
```

```js
    var logout = function() {
        $.post("/logout", function() {
            $("#user").html('');
            $(".unauthenticated").show();
            $(".authenticated").hide();
        })
        return true;
    }
```

The `logout()` function does a POST to `/logout` and then clears the dynamic content

#### Adding a Logout Endpoint

```java
    @Override
    protected void configure(HttpSecurity http) throws Exception {
    // @formatter:off
    http
        // ... existing code here
        .logout(l -> l
            .logoutSuccessUrl("/").permitAll()
        )
        // ... existing code here
    // @formatter:on
}
```

Many JavaScript frameworks have built in support for CSRF, but it is often implemented in a slightly different way than the out-of-the box behaviour of Spring Security.

creates the cookie:

```java
    @Override
    protected void configure(HttpSecurity http) throws Exception {
    // @formatter:off
    http
        // ... existing code here
        .csrf(c -> c
            .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
        )
        // ... existing code here
    // @formatter:on
}
```

#### Adding the CSRF Token in the Client

well need o explicitly add the CSRF token, which you just made available as a cookie from the backend. To make the code a bit simpler, include the `js-cookie` library:

```html
    <script type="text/javascript" src="/webjars/js-cookie/js.cookie.js"></script>
```

```js
$.ajaxSetup({
  beforeSend : function(xhr, settings) {
    if (settings.type == 'POST' || settings.type == 'PUT'
        || settings.type == 'DELETE') {
      if (!(/^http:.*/.test(settings.url) || /^https:.*/
        .test(settings.url))) {
        // Only send the token to relative URLs i.e. locally.
        xhr.setRequestHeader("X-XSRF-TOKEN",
          Cookies.get('XSRF-TOKEN'));
      }
    }
  }
});
```
