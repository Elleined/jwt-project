# jwt-project
Implementing JWT with spring security

# Authentication
- via Sign up and Login (Username and password)
- via OAuth (Social Login)
## Authentication factors
- Single factor: Only requires username and password.
- 2 Factor Authentication (2FA): Requires username and password, security question, and OTP.

## Authentication Techniques
- Password-based
- OTP
- Pin 
- Security questions
- Social Authentication (OAuth)

# Authorization
- Controls what resources does authenticated users can access. We dont want a regular users doing what admins can do right?.
- Works after the user is authenticated via different authentication techniques. It check if the user have access/ permission to the specific operation.
- hasAnyRole() method

## Authorization Techniques
- Role-based
- JWT(JSON Web Token)
- OAuth/ OpenID

# Claims
- Works after authorization of the user via different authorization techniques, a fine grained permission it ensures that users with the same role have the right(claim) to do specific operation, thus providing granular control over what user can access.
- Does this authorized user has the right/ claim to do this specific operation because claim are given to the user by the application.
- hasAnyAuthority() method

## Analogy
- Just like when you have siblings(users), and your parents(application) will give properties(operations) with each one of their daughter/ son(users). Given that you are all their daughter/ son(users) meaning that your siblings doesn't automatically have a right(claim) in your property(operation).

- Just like in family settings you have siblings(users) and you are all living in the same house because you are all daughters/ sons of your parents(application) meaning that you are all authorize to enter the house. But think of claims each one of you has their own room(operation) in the house right? Your siblings does not have automatic right(claim) to enter your room(operation). Because claims is working after a succesaful authorization.

# Workflow
1. Authentication: you will provide username and password and the app will authenticate your credential.
2. Authorization: comes after you have been authenticated not all the users have the authorization to just use all the operations in your app right?. (Role based)
3. Claims: comes after you have been authorized, because not all users with the same role have a right to do specific operations right?.

## Analogy
1. Authentication: You go to the club the bouncers(login) will check you id card(username and password).
2. Authorization: Now you are inside the club. Inside the club there's staff and customers(roles) you don't want the staff drinking alchohol right? and customers are just casually going inside and out in the admin office right?.
3. Claims: Now that you are inside the club, suppose you are the staff, and staff we all know theres an heirarchy pf position the staff boss and regular staff and we dont want the regular staff just casually signing an important contract right?. which only the staff boss are allowed to do.

# Three parts of JWT
- Header: This part identifies the type of encryption and other data needed to decode the token

- Payload: The data you've encoded into the token like the username, maybe a user role, or a boolean that they are logged in (whatever you want).

- Signature: This is the unique signature that is paired with a Secret Key. If the same key isn't used to decode the string, it won't decode correctly. (The application uses the same secret key for all tokens, the user never knows this exists)

# Workflow
![SAVE_20240713_215346](https://github.com/user-attachments/assets/3146a391-bd24-4e28-af99-ca47a3d7a106)

1. When the user calls the api the first thing that gets executed is the JWTAuthenticationFilter that checks the jwt token that the user has.
2. After the JWTAuthenticationFilter checks and extract the email from the request, if the jwt is present it will call the UserDetailsService to fetch the user details in the database, otherwise will return NoExistingUser.
3. And if the user is exisiting, the next step is, it will call the JWTService that takes two parameters User and jwt token to validate the jwt because one user is one jwt it will checks if the given jwt is match to user fetch jwt. If it matches it will proceed otherwise it will return InvalidJWTToken.
4. Next is spring updates the set user in SecurityContextHolder that the specific user is now authenticated, because earlier we set the user when we call the UserDetailsService that are not yet authenticated so that we need to update to tell spring after so much validation the user is now validated.
5. After bloody validation the DispatcherServlet will now be called and the user request will get executed and function as normal like it intended it to do through Controllers, controllers to service, and so on...


# Useful Links
https://medium.com/@tericcabrel/implement-jwt
-authentication-in-a-spring-boot-3-application-5839e4fd8fac

https://m.youtube.com/watch?v=BVdQ3iuovg0

https://github.com/ali-bouali/spring-boot-3-jwt-security

https://www.javatpoint.com/authentication-vs-authorization

https://github.com/phegondev/users-management-system/blob/java-angular/backend/src/main/java/com/phegondev/usersmanagementsystem/config/JWTAuthFilter.java
