# WCF Endpoints

## What is a WCF Endpoint?

In **Windows Communication Foundation (WCF)**, an **endpoint** is the communication point where messages are sent and received. A WCF service can have multiple endpoints to support various communication methods.

A WCF endpoint consists of three key components:

| Component    | Description                                                                  |
| ------------ | ---------------------------------------------------------------------------- |
| **Address**  | Specifies the location (URI) where the service is hosted.                    |
| **Binding**  | Defines how the communication is carried out (protocol, encoding, security). |
| **Contract** | Specifies what operations the service exposes (interface).                   |

Each endpoint must have a unique **address** and can use different **bindings** to allow multiple communication mechanisms within the same service.

## Structure of a WCF Endpoint

A WCF endpoint is defined using the following format:

```
Address + Binding + Contract = Endpoint
```

Example:

```xml
<endpoint address="http://localhost:8080/Service1"
          binding="wsHttpBinding"
          contract="IService1" />
```

### 1. **Address** (Where the service is hosted)

The **Address** is the URL where the service listens for requests. It consists of the following parts:

```
[protocol]://[domain or IP]:[port]/[servicePath]
```

Examples:
| Protocol | Example Address |
|-----------|----------------|
| HTTP | `http://localhost:8080/Service1` |
| TCP | `net.tcp://localhost:9000/Service1` |
| Named Pipes | `net.pipe://localhost/Service1` |
| MSMQ | `net.msmq://localhost/private/Service1` |

### 2. **Binding** (How the service communicates)

Bindings define how messages are transmitted, including protocol, security, and encoding settings.

| Binding Type            | Use Case                                                             |
| ----------------------- | -------------------------------------------------------------------- |
| **BasicHttpBinding**    | Simple, interoperable web services (ASMX compatibility).             |
| **NetTcpBinding**       | High-performance communication between WCF services.                 |
| **WSHttpBinding**       | Secure, reliable web service communication.                          |
| **NetNamedPipeBinding** | Fast, efficient communication within the same machine.               |
| **MSMQBinding**         | Asynchronous, queued messaging using Microsoft Message Queue (MSMQ). |

### 3. **Contract** (What operations the service provides)

A **contract** is an interface that defines the operations the service exposes. It is marked with `[ServiceContract]` and contains methods marked with `[OperationContract]`.

Example:

```csharp
[ServiceContract]
public interface IService1
{
    [OperationContract]
    string GetData(int value);
}
```

## Defining Endpoints in Configuration (`web.config`)

Endpoints can be configured inside `web.config` for flexible management.

Example:

```xml
<system.serviceModel>
    <services>
        <service name="Service1">
            <endpoint address="" binding="wsHttpBinding" contract="IService1"/>
            <endpoint address="" binding="netTcpBinding" contract="IService1"/>
        </service>
    </services>
</system.serviceModel>
```

## Multiple Endpoints for a Single Service

A WCF service can expose multiple endpoints to allow different clients to communicate using various protocols.

Example:

```xml
<system.serviceModel>
    <services>
        <service name="Service1">
            <endpoint address="http://localhost:8080/Service1" binding="basicHttpBinding" contract="IService1" />
            <endpoint address="net.tcp://localhost:9000/Service1" binding="netTcpBinding" contract="IService1" />
        </service>
    </services>
</system.serviceModel>
```

## Creating Endpoints in Code

Endpoints can also be defined in **C#** instead of configuration files.

Example:

```csharp
using System;
using System.ServiceModel;

[ServiceContract]
public interface IService1
{
    [OperationContract]
    string GetData(int value);
}

public class Service1 : IService1
{
    public string GetData(int value)
    {
        return "You entered: " + value;
    }
}

class Program
{
    static void Main()
    {
        using (ServiceHost host = new ServiceHost(typeof(Service1), new Uri("http://localhost:8080/Service1")))
        {
            host.AddServiceEndpoint(typeof(IService1), new WSHttpBinding(), "");
            host.Open();
            Console.WriteLine("Service is running...");
            Console.ReadLine();
            host.Close();
        }
    }
}
```

## Best Practices for WCF Endpoints

1. **Use multiple endpoints** for flexibility (e.g., **BasicHttpBinding** for external clients and **NetTcpBinding** for internal communication).
2. **Ensure security settings** are correctly configured in bindings (e.g., enable message encryption where needed).
3. **Optimize performance** by selecting the right binding based on network conditions and security requirements.
4. **Clearly document contracts** so that consumers understand what services are available.
5. **Use configuration files** for easy management and updates without modifying code.

---

This document explains **WCF Endpoints** in detail. Other documents will explore **Hosting, Security, Error Handling, and Modern Alternatives** in depth.
