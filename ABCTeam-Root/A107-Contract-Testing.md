
# Contract Testing

Contract testing is a technique used in software development to verify interactions between microservices or components of a distributed system. It focuses on ensuring that services communicate as expected without breaking their contracts or dependencies.

## What is Contract Testing?

Contract testing involves creating and verifying agreements (contracts) between services about their communication protocols, data formats, and behaviors. Unlike traditional end-to-end testing, which tests the entire system, contract testing isolates interactions between services to detect issues early in the development lifecycle.

### Key Concepts:

- **Consumer-Driven Contracts**: Consumers (clients) define expected behaviors and data formats of the service they depend on.
- **Provider-Side Verification**: Providers (services) validate that they adhere to the contracts defined by consumers.
- **Independence**: Contracts are tested independently of the entire system, allowing early detection of integration issues.

## Benefits of Contract Testing:

- **Early Detection of Integration Issues**: Identify issues in service interactions before deployment.
- **Improved Collaboration**: Encourages collaboration between teams by defining clear interfaces.
- **Faster Iterations**: Enables faster iterations and deployments by reducing integration-related risks.
- **Reduced Dependencies**: Minimizes dependencies on shared environments or external systems during testing.

## Tools and Frameworks:

Several tools and frameworks support contract testing:

- **Pact**: Allows consumer-driven contract testing for HTTP and message-based integrations.
- **Spring Cloud Contract**: Integrates with Spring-based applications to define and verify contracts.
- **WireMock**: Simulates HTTP-based APIs for contract testing in isolation.
- **Mountebank**: Provides cross-platform support for creating stubs and mocks to verify contracts.

## Contract Testing Workflow:

1. **Define Contracts**: Consumers specify expected interactions and data formats in contract files (e.g., Pact JSON format).

2. **Implement Contracts**: Providers implement APIs or services that adhere to the defined contracts.

3. **Verify Contracts**: Providers verify their implementations against the contracts defined by consumers using automated tests.

4. **Publish Contracts**: Contracts are published to a shared repository or artifact repository for continuous integration.

5. **Run Tests**: Automated tests validate interactions between services, ensuring compliance with contracts.

## Best Practices:

- **Clear Definitions**: Ensure contracts are clear, precise, and agreed upon by both consumers and providers.
- **Versioning**: Manage contract versions to handle changes and ensure backward compatibility.
- **Continuous Integration**: Integrate contract tests into CI/CD pipelines for early feedback.
- **Collaboration**: Foster collaboration between development teams to maintain and evolve contracts.



Implementing contract testing with Pact in a Spring Boot application involves setting up both the consumer side (client) and the provider side (service). Pact allows you to define and verify contracts between these components. Here’s a step-by-step example of how to implement Pact contract testing in a Spring Boot application:

### Consumer Side (Client)

#### Step 1: Add Dependencies

Add necessary dependencies to your consumer project’s `pom.xml` (if using Maven) or `build.gradle` (if using Gradle):

**Maven:**
```xml
<dependency>
    <groupId>au.com.dius</groupId>
    <artifactId>pact-jvm-consumer-spring_2.13</artifactId>
    <version>4.2.0</version>
    <scope>test</scope>
</dependency>
```

**Gradle:**
```gradle
testImplementation 'au.com.dius:pact-jvm-consumer-spring_2.13:4.2.0'
```

#### Step 2: Write Pact Consumer Test

Create a Pact consumer test to define the expected interactions and contract with the provider.

```java
import au.com.dius.pact.consumer.dsl.PactDslWithProvider;
import au.com.dius.pact.consumer.junit5.PactConsumerTestExt;
import au.com.dius.pact.consumer.junit5.PactTestFor;
import au.com.dius.pact.model.RequestResponsePact;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.http.HttpMethod;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;

import java.io.IOException;

import static org.junit.jupiter.api.Assertions.assertEquals;

@ExtendWith(PactConsumerTestExt.class)
@PactTestFor(providerName = "ProviderService") // Name of the provider service
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class ContractConsumerTest {

    // Define the interaction with the provider
    @Pact(provider = "ProviderService", consumer = "ConsumerService") // Name of the provider and consumer
    public RequestResponsePact createPact(PactDslWithProvider builder) {
        return builder
            .given("test GET request")
            .uponReceiving("GET request to /api/resource")
            .path("/api/resource")
            .method(HttpMethod.GET.toString())
            .willRespondWith()
            .status(HttpStatus.OK.value())
            .body("{\"id\": \"1\", \"name\": \"Test Resource\"}") // Expected response body
            .toPact();
    }

    @Test
    @PactTestFor(pactMethod = "createPact")
    public void testProviderService(MockServer mockServer) throws IOException {
        ResponseEntity<String> response = new TestRestTemplate().getForEntity(mockServer.getUrl() + "/api/resource", String.class);
        assertEquals(HttpStatus.OK, response.getStatusCode());
        assertEquals("{\"id\": \"1\", \"name\": \"Test Resource\"}", response.getBody());
    }
}
```

#### Explanation:

- **Dependencies**: Include `pact-jvm-consumer-spring` for Pact consumer tests.
- **Test Annotation**: Use `@PactTestFor` and `@ExtendWith(PactConsumerTestExt.class)` to define the Pact test.
- **Pact Definition**: Use `PactDslWithProvider` to define the expected interaction (GET request to `/api/resource`).
- **Test Method**: Use `TestRestTemplate` to make a request to the mock server (`mockServer.getUrl()`).
- **Assertions**: Verify the response status code and body against the expected values.

### Provider Side (Service)

#### Step 1: Add Dependencies

Add necessary dependencies to your provider project’s `pom.xml` or `build.gradle`:

**Maven:**
```xml
<dependency>
    <groupId>au.com.dius</groupId>
    <artifactId>pact-jvm-provider-spring_2.13</artifactId>
    <version>4.2.0</version>
    <scope>test</scope>
</dependency>
```

**Gradle:**
```gradle
testImplementation 'au.com.dius:pact-jvm-provider-spring_2.13:4.2.0'
```

#### Step 2: Write Pact Provider Test

Create a Pact provider test to verify that the provider adheres to the contract defined by the consumer.

```java
import au.com.dius.pact.provider.junit5.HttpsTestTarget;
import au.com.dius.pact.provider.junit5.PactVerificationContext;
import au.com.dius.pact.provider.junit5.Provider;
import au.com.dius.pact.provider.junit5.ProviderType;
import au.com.dius.pact.provider.junit5.VerificationReports;
import au.com.dius.pact.provider.junit5.VerificationRunner;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.TestTemplate;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.web.server.LocalServerPort;
import org.springframework.test.context.junit.jupiter.SpringExtension;

@ExtendWith(SpringExtension.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@Provider("ProviderService") // Name of the provider service
@VerificationReports({"console"})
public class ContractProviderTest {

    @LocalServerPort
    private int port;

    @BeforeEach
    void before(PactVerificationContext context) {
        context.setTarget(new HttpsTestTarget("localhost", port, "/"));
    }

    @TestTemplate
    @ExtendWith(VerificationRunner.class)
    void pactVerificationTestTemplate(PactVerificationContext context) {
        context.verifyInteraction();
    }
}
```

#### Explanation:

- **Dependencies**: Include `pact-jvm-provider-spring` for Pact provider tests.
- **Test Annotation**: Use `@Provider` to specify the provider name (`ProviderService`).
- **Test Template**: Use `@TestTemplate` and `@ExtendWith(VerificationRunner.class)` to define the Pact verification test.
- **Verification Context**: Use `PactVerificationContext` to set up the target server (`HttpsTestTarget`) and verify interactions (`context.verifyInteraction()`).

### Running Tests

- Run the consumer tests first to generate the Pact contract file (`target/pacts/consumerService-providerService.json`).
- Use the generated Pact contract file in the provider tests to verify that the provider adheres to the contract.

### Conclusion

Pact contract testing ensures that services communicate correctly by defining and verifying contracts between them. This approach helps in detecting integration issues early and ensuring compatibility between microservices in a distributed system. Adjust the tests and configurations based on your project’s requirements and specific APIs being tested.
Contract testing is a crucial practice in microservices and distributed systems architecture, enabling teams to validate interactions between services efficiently. By defining and verifying contracts early in the development process, teams can reduce integration risks and accelerate delivery while maintaining system reliability.


