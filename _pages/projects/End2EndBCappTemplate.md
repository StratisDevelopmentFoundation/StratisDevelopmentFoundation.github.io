---
title: End to end blockchain web application
permalink: /projects/E2EbcWebApp/
categories: active_projects
short_overview: A simple application to showcase the end to end infrastructure template for a blockchain web application
---
# End to end blockchain web application

## Overview

Here at the Stratis development foundation…
We have set a goal to have 1000 useful applications running on the Stratis blockchain. However, the barrier of entry to developing an end to end blockchain application is high. A developer must possess full stack coding, security, and blockchain technology skills. 

We are making this easier by providing developers with an end to end infrastructure template:
    Secure full node to interact with the blockchain
    Web server for hosting web pages that relate to the blockchain application being created
    Back end data storage for keys, user data, etc
    
We are creating a simple application to showcase the template. 
    It will use smart contracts for an example use case, but can be applied to non smart contract use cases.
    By modifying or replacing the application, developers can make something new and useful.
    The infrastructure decisions are already made, so a full stack developer is not required.

This is important because most of the creativity and uses of the blockchain happen at the application level. By allowing developers to focus on the application instead of worrying about how to set up the infrastructure (servers and connections) around the application, developers can build something useful in a number of hours.  This is a way we can grow the number of useful applications on the Stratis network to meet our goal of 1000 useful apps.

### Components of the application template 

Stratis Full Node
    This piece of software produced by the Stratis team is the core software that provides information about the blockchain, and allows transactions to be written to the blockchain.
    While it is already a self contained system that is easy to install, we will provide additional information about how to configure it for security and usability
Blockchain explorer API
    This is a layer that sits on top of the Stratis Full Node and indexes as much information as possible about the blockchain. Then, when required by applications, it can answer complex questions about what is happening on the blockchain
    This piece of software needs to be developed. We have created specifications for it but are still discussing the exact functionality with the developers who will build it
Application Smart contract (Alias app Smart contract)
    The Smart Contract is a piece of code that runs ON the blockchain, and decentralizes a certain useful component of the overall application.
    In this case, the sample application is an app that allows Addresses on the blockchain to register an Alias, so that other people can send money to them by name instead of by address. While this seems trivial, it leads into a number of other interesting use cases, and provides a good sample application for other developers to modify and build on top of.
Application logic Web app
    This component is a web server and attached database that interacts with the other components. It provides an external web view of what the application is doing, and takes some interaction from users of the application.
    Almost any useful blockchain app will need something similar - a website that presents what is happening on the blockchain in a way that makes sense for that particular app.
    This is where the majority of creativity from future app creators will happen. Users interact with the blockchain mainly through this site.
Packaging
    In order for app creators to make use of this template, they must be able to get it up and running locally on their computer so that they can edit the parts they way to modify.
    Therefore, we need to provide excellent instructions on how to set up everything, and make the setup process as idiot proof as possible.
    Alternately, there may be some ways that we can package up the sample app in a way that is very easy to deploy, depending on what it is hosted on. For example, we could create some kind of Azure Cloud Server image. Investigation is needed on the best way to meet our goals.

## Current Status

Discussion about how to implement this is taking place in the [Discord](/discord/) channel: #sample-app-template 

We will be discussing topics from each component such as: 
Infrastructure:
    What kind of stack should we use and why?
Blockchain explorer API
    What features would be usefull in a blockchain explorer. 
Stratis Full Node
    Best security practices
Application logic Web app
    What should the focus of our sample app be? 
Application Smart contract (Alias app Smart contract)
    How can we design the smart contract to best suit the application? 
Packaging
    How can we best package this application to make it easy for developers to change the application logic and make it their own. 

## Timeline

This initiative is announced on August 23
Discussion about specifications is from August 23-26
Specifications are finalized by August 27
Specifications are made as specific as possible and ready for developers to implement by August 29
Developers begin work August 29
Estimated time to an MVP template: 2-3 weeks

## How to contribute to this project
Join us on [Discord](/discord/) to contribute.
