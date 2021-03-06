# Sypht Kotlin Client
This repository is a Kotlin reference client implementation for working with the Sypht API. [![Docs](https://img.shields.io/badge/API%20Docs-site-lightgrey.svg?style=flat-square)](https://docs.sypht.com)

## About Sypht
[Sypht](https://sypht.com) is a SaaS [API]((https://docs.sypht.com/)) which extracts key fields from documents. For
example, you can upload an image or pdf of a bill or invoice and extract the amount due, due date, invoice number
and biller information.

## Getting started
To get started you'll need API credentials, i.e. a `<client_id>` and `<client_secret>`, which can be obtained by registering
for an [account](https://www.sypht.com/signup/developer)

## Prerequisites
- Install Java 8 JDK or upward

```Bash
brew tap AdoptOpenJDK/openjdk
brew cask install adoptopenjdk8
```
- Install [Kotlin](https://kotlinlang.org)

```Bash
brew install kotlin
```

## Installation
Sypht Kotlin Client is available on maven central

### Maven
```Xml
<dependency>
  <groupId>com.sypht</groupId>
  <artifactId>sypht-kotlin-client</artifactId>
  <version>1.0</version>
</dependency>
```

### Gradle
```Gradle
// https://mvnrepository.com/artifact/com.sypht/sypht-kotlin-client
compile group: 'com.sypht', name: 'sypht-kotlin-client', version: '1.0'
```

## Usage
Create a `SyphtClient` with the credentials generated above. 
You can initialize the `SyphtClient` using environment variables or credentials directly.

```kotlin
// Inject credentials through constructor, this works best for front-end applications
val client = SyphtClient(BasicCredentialProvider("client-id", "client-secret"))
// OR
// Create a client that gets it's credentials from environment variables
// This is the default constructor behaviour.
// This approach *does not* work for front-end environments like Android or web-apps.
// This approach is best suited for server-side code
val client = SyphtClient(EnvironmentVariableCredentialProvider()) 
// same as 
val client = SyphtClient()
```

If you are injecting credentials through environment variables, you have to set the following
environment variables.

```Bash
SYPHT_API_KEY="<client_id>:<client_secret>"
```

or

```Bash
OAUTH_CLIENT_ID="<client_id>"
OAUTH_CLIENT_SECRET="<client_secret>"
```
You can also set the Http Request Timeout using the optional property:
```Bash
REQUEST_TIMEOUT="<value_in_seconds>"
```

then invoke the client with a file of your choice:
```kotlin
val client = SyphtClient()
        println(
                client.result(
                        client.upload(
                                File("receipt.pdf"))))
// OR
val inputStream = File("receipt.pdf").inputStream()
val client = SyphtClient()
        println(
                client.result(
                        client.upload("receipt.pdf", inputStream)))
```

Optionally, you can tell the upload method how to parse the file.

```kotlin
client.upload("receipt.pdf", 
    inputStream, 
    arrayOf(
        "sypht.invoice",
        "sypht.general",
        "sypht.document",
        "sypht.bill",
        "sypht.bank")
)
```

## Testing
Open pom.xml and add the below line inside `<environmentVariables> </environmentVariables>` with the credentials generated above:
```xml
<OAUTH_CLIENT_ID>client_id</OAUTH_CLIENT_ID>
<OAUTH_CLIENT_SECRET>client_secret</OAUTH_CLIENT_SECRET>
<SYPHT_API_KEY>client_id:client_secret</SYPHT_API_KEY>
```
then run 
```Bash
mvn test
```

## License
The software in this repository is available as open source under the terms of the [Apache License](https://github.com/sypht-team/sypht-kotlin-client/blob/master/LICENSE).

## Code of Conduct
Everyone interacting in the project’s codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/sypht-team/sypht-kotlin-client/blob/master/CODE_OF_CONDUCT.md).
