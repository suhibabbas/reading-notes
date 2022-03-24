# Related Resources and Integration Testing

## One-to-Many Relationship

In relational databases, a one-to-many relationship occurs when a parent record in one table can potentially reference several child records in another table.

The opposite of a one-to-many relationship is a many-to-many relationship, in which a child record can link back to several parent records.

**how to work with relationships between entities in Spring Data REST.**

one-to-many relationship is defined using the @OneToMany and @ManyToOne annotations and can have the optional @RestResource annotation to customize the association resource

1. **The Data Model**

    ```java
    @Entity
    public class Book {

    @Id
    @GeneratedValue
    private long id;
    
    @Column(nullable=false)
    private String title;
    
    @ManyToOne
    @JoinColumn(name="library_id")
    private Library library;
    
    // standard constructor, getter, setter
    }
    ```

    Let's add the relationship to the Library class as well:

    ```java
    public class Library {
 
    //...
 
    @OneToMany(mappedBy = "library")
    private List<Book> books;
 
    //...
 
    }
    ```

2. **The Repository**

    creat a BookRepository:

    ```java
    public interface BookRepository extends CrudRepository<Book, Long> { }
    ```

3. **The Association Resources**

    add a book to a library:

    ```java
    curl -i -X POST -d "{\"title\":\"Book1\"}" 
    -H "Content-Type:application/json" http://localhost:8080/books
    ```

    And here is the response from the POST request:

    ```java
    {
        "title" : "Book1",
        "_links" : {
        "self" : {
        "href" : "http://localhost:8080/books/1"
        },
        "book" : {
        "href" : "http://localhost:8080/books/1"
        },
        "bookLibrary" : {
        "href" : "http://localhost:8080/books/1/library"
        }
    }
    }
    ```

    associate the book with the library:

    ```java
    curl -i -X PUT -H "Content-Type:text/uri-list" 
    -d "http://localhost:8080/libraries/1" http://localhost:8080/books/1/library
    ```

    verify the books in the library:

    ```java
    curl -i -X GET http://localhost:8080/libraries/1/books
    ```

    The returned JSON object will contain a books array:

    ```java
    {
        "_embedded" : {
            "books" : [ {
            "title" : "Book1",
            "_links" : {
                "self" : {
                "href" : "http://localhost:8080/books/1"
                },
                "book" : {
                "href" : "http://localhost:8080/books/1"
                },
                "bookLibrary" : {
                "href" : "http://localhost:8080/books/1/library"
                }
            }
            } ]
        },
        "_links" : {
            "self" : {
            "href" : "http://localhost:8080/libraries/1/books"
            }
        }
    }
    ```

    remove an association:

    ```java
    curl -i -X DELETE http://localhost:8080/books/1/library
    ```

## Integration Testing in Spring

[Integration Testing in Spring](https://www.baeldung.com/integration-testing-in-spring)