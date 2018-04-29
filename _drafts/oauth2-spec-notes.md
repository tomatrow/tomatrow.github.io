# Oauth2 Specification Notes 

## Intro

Traditional authentication passed around usernames/passwords. It simple but has drawbacks. 

A better idea is for software clients to ask resource servers directly via temproary tokens. 

* Roles 
    - resource owner: owns the goods 
    - resource server: holds the goods 
    - client: wants the goods 
    - authorization server: lets you have the goods 
* Grants
    - Authorization Code: Lets the client ask for tokens
    - Implicit: directy asks for a token without an Authorization Code
    - Resource Owner Password Credentials: Exchange username/password for a token
    - Client Credentials: Authorization Code, but limited in scope to client controlled things 
* Tokens 
    - Access Token: Encodes permission scope  
    - Refresh Token:
        + Used to obtain Access Token's from an Authorization Server 
        + Only issued with a Access Token
        + Equal or lesser permission/scope than the Access Token it cam with 
* Clients
    - Types 
        + confidential: secure/controlled/private (e.g. private server)
        + public: insecure/uncontrolled (e.g. mobile/browser app)
    - Note. Details:
        + Could be an umbrella over multiple systems/implementations 
    - Profiles (or examples)
        + web application: confidential, tokens are stored on the server 
        + user-agent-based application: public, tokens in the browser 
        + native application: public, tokens on the device 
    - Identifier: Issued for each Authorization Server
    - Authentication 
        + clients need to be authenticated (e.g. key-pair, password) but you *MUST NOT* rely on this for identification 
        + unregistered clients are outside scope 
* Endpoints 
    - Authorization endpoint: Used by Authorization Code and Implicit grant flows to obtain authorization
    - Token endpoint: Used by every flow except Implicit. Exchanges an authorization grants or refresh token for an access token
    - Redirection endpoint
* Scope 
    - 