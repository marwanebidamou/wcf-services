# Error Handling in WCF

## Introduction

Error handling in **Windows Communication Foundation (WCF)** is crucial for maintaining **service reliability, debugging, and security**. WCF provides various mechanisms to **capture, log, and manage exceptions**, ensuring smooth client-service interactions.

### Key Approaches to Error Handling in WCF

| Error Handling Mechanism                  | Purpose                                                    |
| ----------------------------------------- | ---------------------------------------------------------- |
| **Fault Contracts**                       | Enables structured SOAP fault messages for clients.        |
| **Exception Handling in Service Methods** | Handles exceptions locally within service implementations. |
| **IErrorHandler Interface**               | Custom error handling across all operations.               |
| **Logging & Tracing**                     | Captures detailed logs for monitoring and debugging.       |

---

## 1. Handling Errors with Fault Contracts

Fault Contracts allow **SOAP-based WCF services** to return structured error messages instead of generic .NET exceptions.

### Example: Defining a Fault Contract

```csharp
[DataContract]
public class CustomFault
{
    [DataMember]
    public string ErrorMessage { get; set; }

    [DataMember]
    public string ErrorCode { get; set; }
}
```

### Applying Fault Contracts to a Service

```csharp
[ServiceContract]
public interface IService1
{
    [OperationContract]
    [FaultContract(typeof(CustomFault))]
    string GetData(int value);
}

public class Service1 : IService1
{
    public string GetData(int value)
    {
        if (value < 0)
        {
            CustomFault fault = new CustomFault
            {
                ErrorMessage = "Negative values are not allowed.",
                ErrorCode = "400"
            };
            throw new FaultException<CustomFault>(fault);
        }
        return $"You entered: {value}";
    }
}
```

### Handling Faults on the Client Side

```csharp
try
{
    IService1 proxy = new Service1Client();
    string result = proxy.GetData(-1);
}
catch (FaultException<CustomFault> ex)
{
    Console.WriteLine($"Error: {ex.Detail.ErrorMessage}, Code: {ex.Detail.ErrorCode}");
}
```

---

## 2. Using `IErrorHandler` for Global Error Handling

The **`IErrorHandler` interface** allows centralized error handling across all service operations.

### Implementing `IErrorHandler`

```csharp
public class GlobalErrorHandler : IErrorHandler
{
    public bool HandleError(Exception error)
    {
        Console.WriteLine("Logging error: " + error.Message);
        return true; // Indicates error was handled.
    }

    public void ProvideFault(Exception error, MessageVersion version, ref Message fault)
    {
        FaultException faultException = new FaultException("An internal error occurred.");
        MessageFault messageFault = faultException.CreateMessageFault();
        fault = Message.CreateMessage(version, messageFault, faultException.Action);
    }
}
```

### Attaching the Error Handler to the Service

```csharp
public class GlobalErrorBehavior : IServiceBehavior
{
    public void ApplyDispatchBehavior(ServiceDescription serviceDescription, ServiceHostBase hostBase)
    {
        foreach (ChannelDispatcher dispatcher in hostBase.ChannelDispatchers)
        {
            dispatcher.ErrorHandlers.Add(new GlobalErrorHandler());
        }
    }

    public void Validate(ServiceDescription serviceDescription) { }
    public void AddBindingParameters(ServiceDescription serviceDescription, ServiceHostBase hostBase, Collection<ServiceEndpoint> endpoints, BindingParameterCollection bindingParameters) { }
}
```

### Registering the Error Behavior in `web.config`

```xml
<system.serviceModel>
    <behaviors>
        <serviceBehaviors>
            <behavior>
                <serviceMetadata httpGetEnabled="true" />
                <serviceDebug includeExceptionDetailInFaults="false" />
            </behavior>
        </serviceBehaviors>
    </behaviors>
</system.serviceModel>
```

---

## 3. Debugging with Service Debug Behavior

WCF allows enabling exception details for debugging purposes.

### Enabling Exception Details in `web.config`

```xml
<serviceBehaviors>
    <behavior>
        <serviceDebug includeExceptionDetailInFaults="true"/>
    </behavior>
</serviceBehaviors>
```

> **Warning:** Keep `includeExceptionDetailInFaults="false"` in production for security reasons.

---

## 4. Logging and Tracing Errors

WCF supports **logging and tracing** to monitor error details.

### Enabling Logging in `web.config`

```xml
<system.diagnostics>
    <sources>
        <source name="System.ServiceModel" switchValue="Verbose">
            <listeners>
                <add name="xmlListener" type="System.Diagnostics.XmlWriterTraceListener" initializeData="wcf_log.svclog" />
            </listeners>
        </source>
    </sources>
</system.diagnostics>
```

### Viewing Logs

1. Open `wcf_log.svclog` using **Microsoft Service Trace Viewer**.
2. Analyze detailed logs for troubleshooting errors.

---

## 5. Best Practices for WCF Error Handling

1. **Use Fault Contracts** for structured error messages.
2. **Implement `IErrorHandler`** for centralized error handling.
3. **Enable logging and tracing** to monitor errors.
4. **Avoid exposing internal exception details** in production.
5. **Gracefully handle errors on the client side** to prevent crashes.

---

This document explains **WCF Error Handling** in detail. Other documents will cover **Modern Alternatives and Further Reading** in depth.
