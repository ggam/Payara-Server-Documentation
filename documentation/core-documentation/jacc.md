# Core Documentation

JACC stands for Java Authentication Contract for Containers. It's defined in [JSR 115](https://jcp.org/en/jsr/detail?id=115) and all Java EE complaint servers are mandated to provide a default JACC Policy.

## How to install a custom JACC provider

Having coded a JACC provider, the first thing to register it is to make the classes available for the server. For that purpose, you need to put the implementation JAR, with all its dependencies, under `[payara_home]/lib`. So for example, if Payara Server is installed under `/opt/payara`, your JAR should reside under `/opt/payara/lib`. That way it will be part of Payara's global classpath on boot.

The next thing to do is to tell Payara you want to use that specific JACC provider. For that purpose, you have to add the following code to your `domain.xml`:

```xml
<security-service jacc="custom-provider">
    <jacc-provider policy-provider="com.example.CustomPolicy" name="custom-provider" policy-configuration-factory-provider="com.example.CustomPolicyConfigurationFactory"></jacc-provider>
    <!-- More providers can be defined -->
</security-service>
```

That `domain.xml` will correspond to the domain you are using. In case you are using `payaradomain` domain (recommended), it will be located under `[payara_home]/glassfish/domains/payaradomain/config/domain.xml`.

As you can derive from the code excerpt, more named JACC providers can be defined, but only one will be used at runtime, specified by the `jacc` attribute on the `security-service` element.
