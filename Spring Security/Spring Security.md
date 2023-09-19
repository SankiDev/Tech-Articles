# Spring Security

Spring Security is a powerful and highly customizable framework that provides authentication and authorization capabilities for your Java applications. It is designed to secure your application from unauthorized access and protect sensitive data.

# I**mportant concepts in Spring Security**

- Authentication and Authorization
- Confidentiality
- Integrity
- CSRF and CORS

## **Authentication And Authorization** :

Authentication and authorization are the two fundamental concepts in Spring Security. Authentication refers to the process of verifying the identity of a user, while authorization determines what actions a user is allowed to perform within the application. By implementing these concepts, Spring Security ensures that only authenticated and authorized users can access the protected resources.

![Untitled](Spring%20Security%2018accc101f754ebbb357dda30337f8a9/Untitled.png)

### **Authentication :**

It is the process of letting any application know who we are. It is a crucial aspect of application security. It ensures that only authorized users can access protected resources and perform actions within the application.

For example, login to banking application using username and password : The banking application will validate the username and password. This process of validation is calling Authentication. If the Authentication succeeds, only then the user can login to the application, otherwise not.

### **Importance of Authentication :**

Authentication is the process of verifying the identity of a user before granting access to the application. It is essential for protecting sensitive data and preventing unauthorized access. Without proper authentication measures in place, anyone could potentially gain access to confidential information or perform malicious actions within your application

### **Different ways of Authentication :**

- Basic Authentication
- Form Based Authentication
- OAuth (Single Sign On in Rest applications)

### **Authorization :**

Once a use logs in, Authorization is used to determine how much access a user has in the application. It uses Roles to do Authorization. Each Role can be mapped to certain urls, or methods in the application, and the user with certain role will have access to certain functionality within the application.

**Note :** We can also create our own Custom Login mechanism instead of using Basic , Form or OAuth. We can create our own Authentication and Authorization process as well.

## **Confidentiality :**

Application ensures that data they are sharing is not vulnerable to hackers.

- Encryption and decryption of data using https
- Once communication is encrypted, even if the hacker captures the exchanged data, he will not be able to make much sense out of the data as it is encrypted. The sender application will use public key along with the data to send to the receiver. The receiver app will need to use a private key to decrypt the sent data. So the hacker will not be able to do anything with sent data unless he has access to those private keys.

![Untitled](Spring%20Security%2018accc101f754ebbb357dda30337f8a9/Untitled%201.png)

## **Integrity :**

Applications ensure whatever data is exchanged is really coming from same user/application it is expecting, and the data is not changed during the data exchange process. ie, if a hacker manages to capture the data during its exchange between sender and receiver, and is able to put something else in that data and re-sends the data to the receiver application, the receiver application should be able to know if the data is tweaked and integrity of data is maintained or not. Signatures help in maintaining this integrity. 

Authorization server and resource server participates in the process of validating signatures to help maintain data integrity.

**Authorization Server :** The Authorization Server generates an access token, signs the token using a private key and gives it back to the application. The application sends the token to the resource server.

**Resource Server :** When a client presents a token to the Resource Server, the Resource Server checks the token's signature to verify its authenticity. If the signature is valid, the Resource Server can trust the token's integrity and proceed with authorization.

![Untitled](Spring%20Security%2018accc101f754ebbb357dda30337f8a9/Untitled%202.png)

## **CSRF (Cross Site Request Forging) and CORS (cross origin resource sharing):**

**CSRF :** is a security vulnerability that occurs when an attacker tricks a user into unknowingly making unauthorized and potentially harmful requests to a web application where the user is authenticated

**Cross-Origin Resource Sharing (CORS) :**  is a security feature implemented by web browsers to control how web pages in one domain can make requests for resources (e.g., data, scripts, or images) from a different domain. CORS ensures that such cross-origin requests are only permitted if the target server explicitly allows them. It is used to prevent potential security vulnerabilities like unauthorized data access or unintended information disclosure between different web domains. CORS policies are defined on the server-side and enforced by the browser to enhance web security and protect user data.

![Untitled](Spring%20Security%2018accc101f754ebbb357dda30337f8a9/Untitled%203.png)

# Key components in Spring Security :

Spring Security consists of several key components that work together to provide a secure environment for your application. 

- **AuthenticationFilter**
- **AuthenticationManager**
- **AuthenticationProvider**
- **UserDetailsService**
- **PasswordEncoder**
- **AuthenticationSuccessHandler**
- **SecurityContext**
- **AuthenticationFailureHandler**

## Flow of Authentication process involving the key components :

![Untitled](Spring%20Security%2018accc101f754ebbb357dda30337f8a9/Untitled%204.png)

- When a client sends a request to the protected resources, **AuthenticationFilter** intercepts the incoming requests, extract the authentication credentials, and initiate the authentication process. **AuthenticationFilter** is a servlet class that will see if the user has authenticated.
- If user is not authenticated, **AuthenticationFilter** will send the request to the **AuthenticationManager** to check if the details (username and password) sent by the user are correct.
- **AuthenticationManager** in turn uses **AuthenticationProvider.** The Authentication logic is defined in **AuthenticationProvider**.
- **AuthenticationProvider** use **User Details Service** service to fetch the user details (from LDAP, database, in-memory or authorization server of OAuth). **AuthenticationProvider** also uses **PasswordEncoder** to encode the password coming from user and do the password comparison, instead of using plain text.
- Once **AuthenticationProvider** checks if the authentication details coming from user (username , password, etc) are correct, it will send the appropriate response back to the **AuthenticationManager**.
- **AuthenticationManager** hands the received response back to the **AuthenticationFilter**.
- If the user details are okay, the authentication succeeds and **AuthenticationFilter** will use an **AuthenticationSuccessHandler** to store the authentication information (User information) in a **Security Context.**
- If Authentication fails, **AuthenticationFilter** uses **AuthenticationFailureHandler** to send the appropriate response back to the client.