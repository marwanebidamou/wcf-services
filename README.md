# Windows Communication Foundation (WCF) Guide

## Overview

**Windows Communication Foundation (WCF)** is a **legacy** technology primarily used for **service-oriented applications** in .NET Framework. While WCF is **not supported in .NET 5 and later**, it is still widely used in **many existing enterprise applications**. Due to its continued presence in the market, I created this repository as an **introduction and recap** for anyone needing to understand, maintain, or migrate WCF services.

This guide covers WCF fundamentals, security, hosting, error handling, and modern alternatives, ensuring developers have a comprehensive reference for working with WCF or transitioning to newer technologies.

---

## Table of Contents

1. [Introduction to WCF](#introduction-to-wcf)
2. [WCF Endpoints](#wcf-endpoints)
3. [Hosting WCF Services](#hosting-wcf-services)
4. [WCF Security](#wcf-security)
5. [Error Handling in WCF](#error-handling-in-wcf)
6. [Modern Alternatives to WCF](#modern-alternatives-to-wcf)
7. [Further Reading](#further-reading)

---

## 1. Introduction to WCF

**[Introduction to WCF](./wcf_introduction.md)** covers the core concepts of WCF, explaining:

- What WCF is
- Key features and advantages
- WCF’s architecture and components
- Basic service implementation example

📖 Read more: [Introduction to WCF](./wcf_introduction.md)

---

## 2. WCF Endpoints

**[WCF Endpoints](./wcf_endpoints.md)** explains:

- What endpoints are in WCF
- Structure of an endpoint (Address, Binding, Contract)
- Different types of bindings
- Configuring endpoints in `web.config` and code
- Best practices for defining endpoints

📖 Read more: [WCF Endpoints](./wcf_endpoints.md)

---

## 3. Hosting WCF Services

**[Hosting WCF Services](./wcf_hosting.md)** covers:

- Different hosting options (IIS, Self-Hosting, Windows Service, Azure Hosting)
- Configuring hosting in `web.config`
- Running a WCF service in a console application
- Choosing the right hosting approach

📖 Read more: [Hosting WCF Services](./wcf_hosting.md)

---

## 4. WCF Security

**[WCF Security](./wcf_security.md)** focuses on:

- Security modes (Transport, Message, Mixed)
- Authentication methods (Windows, Username, Certificate-based)
- Authorization and role-based security
- Configuring security in `web.config`
- Best security practices for WCF

📖 Read more: [WCF Security](./wcf_security.md)

---

## 5. Error Handling in WCF

**[Error Handling in WCF](./wcf_error_handling.md)** provides insights on:

- Using Fault Contracts to structure error messages
- Implementing `IErrorHandler` for global error handling
- Enabling logging and tracing for debugging
- Best practices for error handling

📖 Read more: [Error Handling in WCF](./wcf_error_handling.md)

---

## 6. Modern Alternatives to WCF

**[Modern Alternatives to WCF](./wcf_modern_alternatives.md)** explores:

- Why WCF is not supported in .NET 5+
- Transitioning to **ASP.NET Core Web API** for RESTful services
- Using **gRPC** for high-performance RPC communication
- Implementing **SignalR** for real-time communication
- Serverless computing with **Azure Functions** and **AWS Lambda**
- Migration strategies from WCF

📖 Read more: [Modern Alternatives to WCF](./wcf_modern_alternatives.md)

---

## 7. Further Reading

For more details on WCF and modern service architectures:

- [Microsoft WCF Documentation](https://learn.microsoft.com/en-us/dotnet/framework/wcf/)
- [ASP.NET Core Web API](https://learn.microsoft.com/en-us/aspnet/core/web-api)
- [gRPC for .NET](https://learn.microsoft.com/en-us/aspnet/core/grpc)
- [SignalR for Real-Time Communication](https://learn.microsoft.com/en-us/aspnet/core/signalr)
- [Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions)

---

## Conclusion

This guide serves as a **reference for both WCF developers and those migrating to modern architectures**. Whether you are maintaining an existing WCF service or planning to transition to **ASP.NET Core Web API, gRPC, or Serverless Computing**, this repository provides all the necessary details.

📌 **For contributions, feedback, or discussions, feel free to open an issue or submit a pull request.** 🚀
