

## Intro 

### Background

When we want to exchange data between a client and server, we use URI's and any kind of data format. Various abstractions have been built to make these transactions well defined. Examples include SOAP, WDSL, and REST as the current favorite. People wanted a machine/human readable format to define transaction abstractions(haha with the bonehead guy), so we have DSL's just or this purpose build on top of popular markup languages. 

### OpenAPi

#### What is OpenAPi? 

Swagger (now OpenAPI) is a DSL on top of YAML used to describe RESTful API's. 

It's backed by a subsidiary of the Linux Foundation called the Open Api Foundation. 

#### Why OpenAPi? 

High level API prose hides errors. 

Well defined API means we can transform it into other useful forms, like a live test, generated documentation to a target audience (e.g. internal/external users), generated tests, avoiding breaking changes by easy comparison. 

Specifically, OpenAPI allows extensions with a `x-` prefix. 

## Case Study: Profiles for Veteran Owned Businesses 

### Background 

Someone has agreed to invest their time in me. A lot of it. By this, I mean he is giving me tasks in the field I want to be a part of, despite my experience. 

It's taken me weeks to figure out developer/client communication. But, he has remained patient. 

This is the first task given. 

### Description

* Summary: Develop an API large companies can use to fill out their procurement/contracting profiles for smaller companies. 
* Scope: Small veteran owned business in the US. 
* Implementation Process:
    - [ ] 1) JSON Schema of Profiles
    - [ ] 2) OpenAPI Description for Retrieval 
    - [ ] 3) Backend
    - [ ] 4) Frontend

<!-- More Info -->

[1]: https://stackoverflow.com/a/35030135 "Dealing with optionals"
[3]: https://github.com/mermade/awesome-openapi3 "Awesome OpenAPI"
[4]: https://swagger.io/blog/difference-between-swagger-and-openapi/ "Swagger vs OpenAPI"

<!-- Specs  -->

[2]: https://github.com/OAI/OpenAPI-Specification "OpenAPI"
[5]: http://spec.commonmark.org "MD in OpenAPI"
[6]: https://tools.ietf.org/html/draft-wright-json-schema-validation-00 "Superset of OpenAPI's JSON Spec (except for the extensions)"
[7]: https://tools.ietf.org/html/rfc6749 "Oauth2"
