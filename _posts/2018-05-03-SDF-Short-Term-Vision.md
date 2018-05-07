---
title:  "Short Term Vision for the SDF"
date:   2018-05-03 01:01:01 -0600
categories: blog
toc: true
toc_label: ""
toc_icon: "x"
read_time: true
excerpt: "Discussion of what the SDF will be working on over the next 1-2 months."
---

# Short Term Vision of the Stratis Development Foundation

Let's discuss what our goals are in the timeframe from May - June, 2018.

## Technology Goals

We are building articles on how to perform the most common tasks on Stratis. We are also looking to identify what infrastructure is needed for the kinds of solutions people are trying to build on Stratis. In particular, there is a common setup that many applications will need.

A typical application that is built to interact with the Stratis platform has 3 parts. It has a Stratis Full Node running, it has an application that is running business logic and interacting with the blockchain, and it has a web page serving component. These 3 components could be separated out into their own servers, or they could exist on the same server.

As an example, if you had a voting smart contract, even though the deployed smart contract would not need a server because it would run on the blockchain, it is typical that you would still need a server to present the voting results. You would have the Stratis full node part that inspects the state of the smart contract, and provides the data for processing. You would have the application running business logic (in this case, tabulating the votes to figure out who the winner is). You would also have a web app serving up the results of the vote to your audience.

In particular, we want to simplify this common scenario for people who want to build on the Stratis platform. So, we want to have step by step instructions for how to set up the server infrastructure (for example, on Azure), and how to configure it. That way, after following that guide, they can focus on the interesting and exciting part of blockchain development - Their smart contracts, and how their application logic works. However, if we don't provide these instructions, the barrier to entry is much higher - the average developer who wants to create an end to end app on stratis needs to understand setting up cloud servers, setting up something to serve web pages, configuring the Stratis full node, and making an application that interacts with the web server and Stratis full node.

A primary goal of ours is to clearly document common development tasks so that the Stratis developer can be creative with their solutions, rather than struggling with a new technology that may be unfamiliar to them. Articles and server templates will be our focus in achieving this goal.

## Community Structure

The community is centered around building things on Stratis. We support and reward people who help in these efforts.

At the moment, the community is able to communicate with each other on [Discord](/discord/). The leadership is somewhat centralized among people that have been involved with Stratis for a long time. This is a good approach for the time being, but as we grow the number of people involved, we'll want to evolve. Eventually the leadership of the SDF will be closer to a typical open source project, with people who have earned trust through contributions and seniority taking over the leadership.

## SDF Growth

At this time we are looking for slow and steady growth -  We want to have a flow of new developers coming and giving us feedback on our materials. We don't want to grow in membership too fast, which might make it difficult to manage all the people. We also want more time to polish up our materials so that they are as correct and useful as possible. Therefore at the moment we are not yet in our growth stage, but once we have a good set of materials and our community structure more refined, we'll look to get the word out and recruit more people to help.

## Summary

This is a period of discovery, where we figure out what works. As we continue to grow the SDF, we need to be open to trying new ways of doing things, and to also be open to changing what we are building based upon the needs of the Stratis platform users. Flexibility is key.

Please join our community and help bring your ideas to us, we're interested to hear them.