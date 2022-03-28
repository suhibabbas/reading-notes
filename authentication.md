# Spring Authentication

## Authentication and Access Control

1. **Authentication:** who are you?
2. **Authorization:** “access control” what are you allowed to do? 

## Authentication

the main strategy interface has only one method **AuthenticationManager**

```java
    public interface AuthenticationManager {

        Authentication authenticate(Authentication authentication)
         throws AuthenticationException;
    }
 ```

 1. Return an `Authentication` (normally with `authenticated=true`) if it can verify that the input represents a valid principal.
 2. Throw an `AuthenticationException` if it believes that the input represents an invalid principal.
 3. Return `null` if it cannot decide.

 The most commonly used implementation of `AuthenticationManager` is `ProviderManager`, but it has an extra method to allow the caller to query whether it supports a given `Authentication` type:

 ```java
    public interface AuthenticationProvider {

        Authentication authenticate(Authentication authentication)
                throws AuthenticationException;

        boolean supports(Class<?> authentication);
    }
```

![authentication](./img/authentication/authentication.png)


## Authorization or Access Control

here are three implementations provided by the framework and all three delegate to a chain of `AccessDecisionVoter` instances

```java
boolean supports(ConfigAttribute attribute);

boolean supports(Class<?> clazz);

int vote(Authentication authentication, S object,
        Collection<ConfigAttribute> attributes);
```

## Web Security

the typical layering of the handlers for a single HTTP request:

![filters](./img/authentication/filters.png)

The client sends a request to the application, and the container decides which filters and which servlet apply to it based on the path of the request URI.

## Spring Auth cheat sheet

[Spring Auth cheat sheet](https://github.com/codefellows/seattle-java-401d2/blob/master/SpringAuthCheatSheet.md)
