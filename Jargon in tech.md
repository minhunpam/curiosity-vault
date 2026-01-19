## Business Logic #Jargon
- custom rules/ algorithms that define how a business application/ system behaves according to **real-world business rules and processes**
- business logic makes an application behave like the real-world version of the business it supports
### Example: banking system:
- check if the user has enough money before allowing a transfer
- limiting daily transfer amounts
- triggering fraud alerts for unusually large transactions

## Bootstrapping #Jargon
- automatically setting up, initializing and wiring a component so you don't have to do it manually

## _Endpoint_ in Spring (and API)
- An **endpoint** is basically:
> A URL(path) + HTTP method that your application exposes, which clients can call

## URI vs URL vs URN
![[CON - URI_hierarchy_illustration.png]]
### **URI: Uniform Resource Identifier** 
- a sequence of characters that identify a name or a unique resource on the Internet
	- ***In HTTP/ REST APIs, URIs are addresses for your resources***
		- examples: `"api/todos/1"`

#### URI Syntax
```
scheme:[//authority][/path][?query][#fragment]
```
- **scheme**
	- for URLs: name of the protocol used to access the resource
	- for other URIs: name that refers to a specification for assigning identifiers within that scheme
- **authority** - an optional part comprised of user authentication information, a host and an optional port
```
https://localhost:8080/
```
- host: localhost
- port: 8080

* **path** - serves to identify a resource within the scope of its scheme and authority
* **query** - additional data that, along with the _path_, serves to identify a resource
	* For URLs, this is the query string
- **fragment** - an optional identifier to a specific part of the resource
#### URI Template
- URI -> a concrete address
```bash
/users/42
```
- URI template -> a pattern for a URI that contains placeholders (variables)
```
/users/{id}
```
- `{id}` is a variable placeholder
### **URL: Uniform Resource Identifier** 
- defined as a string of characters that is directed to an address
- **URN: Uniform Resource Name** - identifies a resource by its name rather than its location 
![[CON - URI_hierarchy_illustration.png]]

### How to identify if a particular URI is also a URL, we can check its scheme
- URL:
```
ftp, http, https, gopher, mailto, news, nntp, telnet, wais, file, prospero
```
Examples:
```
ftp://ftp.is.co.za/rfc/rfc1808.txt https://tools.ietf.org/html/rfc3986 
mailto:john@doe.com
```

- URI: others
Examples:
```
tel:+1-816-555-1212 urn:oasis:names:docbook:dtd:xml:4.1 
urn:isbn:1234567890
```
## Paradigm
- a way of thinking about and approaching problems
- a programming paradigm is a style or philosophy of writing and organizing code
- There are a few programming paradigms:
	1. Imperative Programming
	2. Declarative Programming
	3. Object-oriented Programming
	4. Functional Programming
	5. Procedural Programming
	6. Logic Programming

## What is DST (Daylight Saving Time) Issue?
### 1. DST?
- In many countries, clocks are:
	- moved forward by 1 hour in spring - On the last Sunday of March: Berlin jumped from **01:59 -> 03:00 (02:00 never existed)** 
	- moved back by 1 hour in autumn: - On the last Sunday of October: Berlin jumped from **02:59 -> 02:00 again (the hour from 02:00 - 02:59 happened twice**
### 2. DST issue in programming?
1. Non-existent times
2. Ambiguous times
3. Offset mistakes
	- Adding 24 hours to a date might not land you at “tomorrow, same time” — because the day might be 23 or 25 hours long.
4. Database/ API mismatches
## Heuristic?
- a **rule-of-thumb** or **practical guideline** that works well in practice,  
but is **not guaranteed to be optimal or provably best in all cases**.
## Throughput?
- Amount of completed work per time
- **In thread pools/ concurrency**: how fast does my thread pool get things done overall?