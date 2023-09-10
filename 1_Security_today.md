# Intro
Nonfunctional software qualities such as performance, scalability, availability, and, of course, security, as well as others, can have an impact over time, from short to long term. 

If not taken into consideration early on, these qualities can dramatically affect the profitability of the application owners. Moreover, the neglect of these considerations can also trigger failures in other systems as well (for example, by the unwilling participation in a `distributed denial of service (DDoS) attack`). The hidden aspects of nonfunctional requirements (the fact that it’s much more challenging to see if something’s missing or incomplete) makes these, however, more dangerous.

There are multiple nonfunctional aspects to consider when working on a software system. In practice, all of these are important and need to be treated responsibly in the process of software development. In this book, we focus on one of these: security.

But before starting, I’d like to make you aware of the following: depending on how much experience you have, you might find this chapter cumbersome. Don’t worry too much if you don’t understand absolutely all the aspects for now. In this chapter, I want to show you the big picture of security-related concepts. Throughout the book, we work on practical examples, and where appropriate, I’ll refer back to the description I give in this chapter. Where applicable, I’ll also provide you with more details. Here and there, you’ll find references to other materials (books, articles, documentation) on specific subjects that are useful for further reading.

## 1.1 Spring Security: The what and the why

In this section, we discuss the relationship between Spring Security and Spring.

If we go to the official website, https://spring.io/projects/spring-security, we see Spring Security described as a powerful and highly customizable framework for authentication and access control. I would simply say it is a framework that enormously simplifies applying (or “baking”) security for Spring applications.

Spring Security is the primary choice for implementing application-level security in Spring applications. Generally, its purpose is to offer you a highly customizable way of implementing authentication, authorization, and protection against common attacks. Spring Security is an open source software released under the Apache 2.0 license. You can access its source code on GitHub at https://github.com/spring-projects/ spring-security/. I highly recommend that you contribute to the project as well.

Note You can use Spring Security for both standard web servlets and reactive applications. To use it, you need at least Java 8, although the examples in this book use Java 11, which is the latest long-term supported version.

Technically, applying security with Spring Security in Spring applications is simple. You’ve already implemented Spring applications, so you know that the framework’s philosophy starts with the management of the Spring context. You define beans in the Spring context to allow the framework to manage these based on the configurations you specify. And you use only annotations to make these configurations and leave behind the old-fashioned XML configuration style!

You use annotations to tell Spring what to do: expose endpoints, wrap methods in transactions, intercept methods in aspects, and so on.

The same is true with Spring Security configurations, which is where Spring Security comes into play. What you want is to use annotations, beans, and in general, a Spring-fashioned configuration style comfortably when defining your application-level security. In a Spring application, the behavior that you need to protect is defined by `methods`.

It’s a puzzle that offers plenty of choices for building the exact image that describes your system. You can choose to leave your house completely unsecured, or you can decide not to allow everyone to enter your home _(home -> your application. Home Sweat Home)_.

The way you configure security can be straightforward like hiding your key under the rug, or it can be more complicated like choosing a variety of alarm systems, video cameras, and locks. In your applications, you have the same options, but as in real life, the more complexity you add, the more expensive it gets. In an application, this cost refers to the way security affects maintainability and performance.


Other responsibilities of Spring Security components relate to data storage as well as data transit between different parts of the systems. By intercepting calls to these different parts, the components can act on the data. For example, when data is stored, these components can apply encryption or hashing algorithms. The data encodings keep the data accessible only to privileged entities. In a Spring application, the developer has to add and configure a component to do this part of the job wherever it’s needed. Spring Security provides us with a contract through which we know what the framework requires to be implemented, and we write the implementation according to the design of the application. We can say the same thing about transiting data.

In real-world implementations, you’ll find cases in which two communicating components don’t trust each other. How can the first know that the second one sent a specific message and it wasn’t someone else? Imagine you have a phone call with somebody to whom you have to give private information. How do you make sure that on the other end is indeed a valid individual with the right to get that data, and not somebody else? For your application, this situation applies as well. Spring Security provides components that allow you to solve these issues in several ways, but you have to know which part to configure and then set it up in your system. This way, Spring Security intercepts messages and makes sure to validate communication before the application uses any kind of data sent or received.

    1. You use Spring Security to bake application-level security into your applications in the “Spring” way. By this, I mean, you use annotations, beans, the Spring Expression Language (SpEL), and so on.
        ```

        The Spring Expression Language (SpEL) is a powerful expression language that supports querying and manipulating an object graph at runtime. The language syntax is similar to Unified EL but offers additional features, most notably method invocation and basic string templating functionality1. SpEL was created to provide the Spring community with a single well-supported expression language that can be used across all the products in the Spring portfolio1. Its language features are driven by the requirements of the projects in the Spring portfolio, including tooling requirements for code completion support within the Spring Tools for Eclipse1. While SpEL serves as the foundation for expression evaluation within the Spring portfolio, it is not directly tied to Spring and can be used independently
    ```   

## 1.2 What is software security?

Software systems today manage large amounts of data, of which a significant part can be considered sensitive, especially given the current `General Data Protection Regulations (GDPR)` requirements.
    ```
    The General Data Protection Regulation (GDPR) is a regulation of the European Union (EU) on information privacy in the EU and the European Economic Area (EEA). It aims to enhance individuals’ control and rights over their personal information and to simplify the regulations for international business. Some of the key privacy and data protection requirements of the GDPR include:

    Requiring the consent of subjects for data processing
    Anonymizing collected data to protect privacy
    Providing data breach notifications
    Safely handling the transfer of data across borders
    Requiring certain companies to appoint a data protection officer to oversee GDPR compliance.
    
```
_NOTE GDPR created a lot of buzz globally after its introduction in 2018. It generally represents a set of European laws that refer to data protection and gives people more control over their private data. GDPR applies to the owners of systems having users in Europe. The owners of such applications risk significant penalties if they don’t respect the regulations imposed._

Security is a complex subject. In the case of a software system, security doesn’t apply only at the application level. For example, for networking, there are issues to be taken into consideration and specific practices to be used, while for storage, it’s another discussion altogether. Similarly, there’s a different philosophy in terms of deployment, and so on.
    
Spring Security is a framework that belongs to application-level security. In this section, you’ll get a general picture of this security level and its implications.

## 1.4 Common security vulnerabilities in web applications

Before we discuss how to apply security in your applications, you should first know what you’re protecting the application from. To do something malicious, an attacker identifies and exploits the vulnerabilities of your application. We often describe vulnerability as a weakness that could allow the execution of actions that are unwanted, usually done with malicious intentions.

An excellent start to understanding vulnerabilities is being aware of the Open Web Application Security Project, also known as OWASP (https://www.owasp.org). At OWASP, you’ll find descriptions of the most common vulnerabilities that you should avoid in your applications. Let’s take a few minutes and discuss these theoretically before diving into the next chapters, where you’ll start to apply concepts from Spring Security. Among the common vulnerabilities that you should be aware of, you’ll find these:

### Broken authentication

Authentication represents the process in which an application identifies someone trying to use it. When someone or something uses the app, we want to find their identity so that further access is granted or not. In real-world apps, you’ll also find cases in which access is anonymous, but in most cases, one can use data or do specific actions only when identified. Once we have the identity of the user, we can process the authorization.

Authorization is the process of establishing if an authenticated caller has the privileges to use specific functionality and data. For example, in a mobile banking application, most of the authenticated users can transfer money but only from their account.




### Session fixation

Session fixation vulnerability is a more specific, high-severity weakness of a web application. If present, it permits an attacker to impersonate a valid user by reusing a previously generated session ID. This vulnerability can happen if, during the authentication process, the web application does not assign a unique session ID. This can potentially lead to the reuse of existing session IDs. Exploiting this vulnerability consists of obtaining a valid session ID and making the intended victim’s browser use it.

        Cross-site scripting (XSS)

        Cross-site request forgery (CSRF)

        Injections

        Sensitive data exposure

        Lack of method access control

        Using dependencies with known vulnerabilities
    
