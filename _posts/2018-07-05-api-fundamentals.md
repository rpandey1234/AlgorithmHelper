---
layout: post
title:  API Fundamentals
author: Rahul Pandey
date:   2018-07-05
categories: api rest
---

An API is an application programming interface. It defines a way for applications to talk to each other to exchange data. APIs are commonly used when discussing how clients can make requests to a web server, but they are used beyond internet-based apps. Libraries and other systems also expose an API which clients must understand. In this discussion, however, we will focus on RESTful API concepts, which are APIs that deal with resources. 

REST APIs are most common, both in the real world and in interview questions. One useful attribute of REST APIs are that calls are stateless, which means that the server does not save any state between requests. This makes it easy to scale REST APIs to handle load changes by simply sending requests to any availble server. Don't worry about knowing what REST stands for; just know that SOAP is another (less preferred) way of building APIs. 

Contents
===========
- [Resources](#resources)
- [HTTP methods](#http-methods)
- [Safe and Idempotent](#safe-and-idempotent)

## Resources

A resource is any information that can be named. Think of a resource as a noun such as a user, payment, or book. If you're familiar with databases, a resource is roughly an entry in a database table. The resource is the high-level description of the object, and it could contain other resources inside it. A URI is a Uniform Resource Identifier, a way to identify the location of a resource. The idea of a REST API is that the URI represents the resource, and we apply different verbs to this. For example, for the examples above, a REST API may represent the resources as `/users`, `/payments`, or `/books`. 

A resource is **not** a verb or action. For example, the URI `/changeToDoList` is better described as a `PUT` on `/to-do-lists`, and `/listOfVehicles` is better described as a GET on `/vehicles`. According to naming conventions, URIs that represent resources should be plural. 

## HTTP methods

A RESTful API uses the standard HTTP methods: 

- `GET`: Used to retrieve resource representations. Can be used to retrieve one resource via an ID, or multiple resources via query parameters. 
- `PUT`: Used to update an entire resource. Typically this involves the resource ID. The entire contents of the resource should be submitted in the request payload. 
- `POST`: Used to create a new resource. 
- `DELETE`: Used to delete or cancel a resource. 
- `PATCH`: Used to perform a partial update to a particular resource. 

`GET` requests usually accept additional data via query parameters in the URL. One common use for query parameters is to filter the result set returned from the server. For example, in `/search/vehicles?make=Chevorlet&model=Cruze`, the query params represent that we want cars with make of Chevorlet and model of Cruze. The other common use case is pagination for a result set, e.g. `/search/vehicles?make=Chevorlet&page_size=20&page=2`.

The other HTTP methods accept data in request body rather than in the URL itself. The way to think about it: the request body is used for data that is being uploaded to the server, while query params are used for filtering the data. 

## Safe and Idempotent 

**Safe** refers to whether the request is modifying the state on the server. 

**Idempotent** refers to the state of the system after the request has completed. A request is idempotent if clients can make the same call repeatedly while producing the same result. 

Here is a table of each HTTP method we discussed: 

| **Method** | **Safe** | **Idempotent** |
| GET     | yes  | yes |
| PUT     | no   | yes |
| DELETE  | no   | yes |
| POST    | no   | no  |
| PATCH   | no   | no  |

Some things to note here: 

- `GET` doesn't modify any data, so it is both safe and idempotent. All the others are of course unsafe since they create, modify, or delete data. 
- `PUT` and `DELETE` are idempotent because repeatedly making that call will not change the data multiple times. `PUT` requires passing in the whole resource in the request body, and you can only delete a resource once (subsequent calls will be a no-op and should return an error code). 
- `POST` and `PATCH` are not idempotent because they will either continue to make new resources (for `POST`), or could continue to modify a resource for `PATCH` (e.g. we may add a phone number for each patch request).

### Sources

- [Slides by Christian Mamuad](https://docs.google.com/presentation/d/1dqAIIBYMXInd6BlxxpCErlXnAHST9d_VOsnGkCH-hZY/edit#slide=id.p)
- [RESTful API](https://searchmicroservices.techtarget.com/definition/RESTful-API)
- [HTTP RFC 2616](https://www.ietf.org/rfc/rfc2616.txt)
- [URI RFC 3986](http://www.ietf.org/rfc/rfc3986.txt)


