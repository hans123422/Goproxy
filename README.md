Unlimited Proxy Deployment and API Guide

This repository documents practical usage patterns, configuration methods, and API integration examples for working with GoProxy unlimited proxy plans. The focus is on real deployment scenarios including endpoint generation, session handling, sub account management, whitelist configuration, rate control concepts, and usage monitoring.

The goal of this repo is to provide a structured reference for developers, automation engineers, and scraping practitioners.


---

Overview

GoProxy provides API endpoints that allow you to programmatically manage proxies and accounts. Typical use cases include:

Generating proxy endpoints with custom parameters
Choosing residential, mobile, or datacenter proxy types
Selecting geographic targeting such as continent, country, or city
Controlling session behavior (rotating or sticky)
Managing sub accounts and credentials
Configuring IP whitelist rules
Querying account usage and plan details

This guide assumes you already have an active GoProxy account and an API key.

---

Authentication

All API requests require an apiKey header.

Example:

apiKey: YOUR_API_KEY

Without this header, requests will be rejected.

---

Proxy Endpoints

You can retrieve default endpoints or generate customized endpoints.

Retrieve default endpoints:

GET /v1/proxy/endpoints

Generate custom endpoints:

POST /v1/proxy/endpoints-continents-custom

Example request body:

{
"proxyType": 1,
"username": "user1",
"password": "pass123",
"protocol": "socks5",
"continentCode": "EU",
"sessionType": "Sticky",
"sessionTime": 30,
"count": 10
}

Key parameters:

proxyType
1 = Residential
2 = Mobile
3 = Datacenter

protocol
http or socks5

sessionType
Rotating or Sticky

sessionTime
Sticky session duration in minutes

count
Number of endpoints returned

---

Session Management

Rotating session

A new IP is assigned automatically. Suitable for scraping, crawling, and tasks requiring frequent IP changes.

Sticky session

The same IP is maintained for a defined duration. Suitable for login flows, session persistence, and multi step workflows.

Session duration is controlled using sessionTime.

---

Sub Account Management

Sub accounts allow separation of proxy access between users, services, or clients.

Create sub account:

POST /v1/subAccount/add

Example body:

{
"proxyType": 1,
"username": "teamUser",
"password": "password123",
"trafficLimit": "",
"remarks": "internal usage"
}

trafficLimit
Value in MB
Empty value means unlimited

Update sub account:

PATCH /v1/subAccount/edit

You can change password, limits, or remarks.

List sub accounts:

GET /v1/subAccount/listHistory

Useful for tracking usage and managing distributed access.

---

Whitelist IPs

Whitelist allows clients to use proxies without authentication.

Add IP to whitelist:

POST /v1/ipWhite/add

{
"proxyType": 1,
"ipAddress": "123.123.123.123",
"remarks": "server node"
}

List whitelisted IPs:

GET /v1/ipWhite/list

Common for servers, VPS nodes, and fixed IP systems.

---

Account and Usage

You can retrieve account level data.

GET /v1/account/getPlanDetails
GET /v1/account/getUsageDetails

You can also update proxy account settings.

PATCH /v1/account/proxySetup

This may include password updates or traffic limits.

---

Python Example

import requests

url = "[https://api.goproxy.com/go-api/v1/proxy/endpoints-continents-custom](https://api.goproxy.com/go-api/v1/proxy/endpoints-continents-custom)"

headers = {
"apiKey": "YOUR_API_KEY"
}

data = {
"proxyType": 1,
"username": "user1",
"password": "pass123",
"protocol": "http",
"continentCode": "US",
"sessionType": "Sticky",
"sessionTime": 15,
"count": 5
}

response = requests.post(url, json=data, headers=headers)
print(response.json())

---

Summary

This repository serves as a practical reference covering:

Proxy endpoint generation
Session configuration
Sub account creation and control
Whitelist management
API integration examples
Usage and plan monitoring

Designed for developers and operators working with automation, scraping, or network routing scenarios.

---

