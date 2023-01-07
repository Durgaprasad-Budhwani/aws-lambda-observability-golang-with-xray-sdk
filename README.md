
## Go-Based AWS Lambda Observability With AWS X-Ray SDK using AWS SAM CLI (Part 1)

![](https://cdn-images-1.medium.com/max/3840/1*FUK36U86lLlrTHwBDk8vsw.png)

**Observability** is the ability to understand the state of a system by analyzing its outputs. It is an essential concept in computer science and engineering, as it allows you to monitor and understand the behaviour of complex systems, identify problems, and fix them before they cause significant issues.

There are three main pillars of observability:

1. **Metrics**: Metrics are numerical values that can be collected and monitored over time. They can measure various aspects of a system’s behaviour, such as performance, resource utilization, and errors.

2. **Logs**: Logs are records of events that happen within a system. They can be used to understand the sequence of events that led to a particular problem and to identify trends and patterns over time.

3. **Tracing**: Tracing involves tracking the flow of a request as it travels through a system, from the point of origin to the final destination. It allows you to understand how different system components interact and can be used to identify issues with specific parts of the system.

Using a combination of metrics, logs, and tracing, you can gain a deep understanding of the behaviour of your systems and identify issues before they become significant problems.

In this article, you will learn how to achieve observability in AWS Lambda when the lambda runtime is [**Golang](https://go.dev/) **programming language**.**

## Prerequisite

To get started, you must have the following prerequisites:

* [AWS Account](https://portal.aws.amazon.com/) with AWS Lambda creation access

* [Golang Install](https://go.dev/dl/)

* [AWS CLI ](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)Configure

* [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html) Install

* [Goland](https://www.jetbrains.com/go/) or VSCode

## Getting Started:

The easiest way to start writing Lambda functions with Golang is by using AWS SAM CLI. It is a command-line tool that you can use to build and test serverless applications defined with AWS Serverless Application Model (SAM)

To create a Go sample application using the AWS SAM CLI, you will need to do the following:

1. Please create a new project directory and navigate to it in your terminal.

2. Run the following command to create a new Go application using the AWS SAM CLI:

   sam init --runtime go1.x

![](https://cdn-images-1.medium.com/max/3268/1*zdlaxMfUkJ_yCaJe7wKX_Q.png)

This will create a new directory with the following structure:

![](https://cdn-images-1.medium.com/max/2140/1*U_xK4fI3Xk_iiP-GM7oMZQ.png)

1. The hello_world the directory contains a sample function that you can use as a starting point for your application. You can modify the main.go file to add your code and use the Makefile to build and test your function.

2. The template.yaml The file contains a SAM template that defines your application's resources, such as AWS Lambda functions and Amazon S3 buckets. You can use this file to define the resources needed by your application.

3. The events the directory contains sample event data you can use to test your function. You can modify the event.json file to create different test scenarios for your function.

hello-world/main.go the file includes the AWS Lambda Handler code:

<iframe src="https://medium.com/media/c5ca0e5db4ef4a745d07f793200a504a" frameborder=0></iframe>

The program consists of two main parts:

1. The main function, which is the entry point of the program. It uses the lambda.Start function to start the AWS Lambda runtime and passes the handler function as the function to be executed when the Lambda function is triggered.

2. The handler function, which is the function that will be executed by AWS Lambda when the function is activated. The handler function takes an events.APIGatewayProxyRequest as an input, which contains information about the HTTP request, and returns an, which includes the HTTP response that will be returned to the client.

The handler the function does the following:

1. Makes an HTTP GET request to the DefaultHTTPGetAddress URL using the http.Get function.

2. If the request fails, it returns an empty events.APIGatewayProxyResponse and the error.

3. If the request is successful, it checks the HTTP status code of the response. If the status code is not 200, it returns an empty events.APIGatewayProxyResponse and the ErrNon200Response error.

4. If the status code is 200, it reads the response body using the ioutil.ReadAll function and stores the data in the ip variable.

5. If the ip the variable is empty; it returns an empty events.APIGatewayProxyResponse and the ErrNoIP error.

6. If the ip the variable is not empty; it returns an events.APIGatewayProxyResponse with the body set to a string containing the message "Hello, [IP]", where [IP] is the IP address read from the response body and a status code of 200.

### Local Testing:

sam local start-api is a command that you can use to start an API Gateway locally using the AWS Serverless Application Model (SAM) CLI. This can be useful for testing and debugging your serverless applications locally before deploying them to AWS.

After running sam local start-api command, it will start an API Gateway locally and make it available at the default URL: http://127.0.0.1:3000. You can use the URL [http://127.0.0.1:3000/hello](http://127.0.0.1:3000/hello) to send HTTP requests to your locally-running API Gateway, and it will show you the current IP address:

![](https://cdn-images-1.medium.com/max/2000/1*UaYrZgTzpl6i0ij-1nIPGg.png)

You can also specify a different port number using the --port option. For example:

    https://www.google.com/search?q=9%3A00+PM+IST+to+mountain+time&oq=9%3A00+PM+IST+to+mountain+time&aqs=cawrome..69i57.7088j0j4&sourceid=chrome&ie=UTF-8sam local start-api --port 8000

This will start the API Gateway on port 8000.

### **Deployment: **

Once you have written and tested your function, you can use the AWS SAM CLI to package and deploy your application to AWS. To do this, run the following command:

    sam build && sam deploy --guided

This will package your application and deploy it to AWS, creating all the necessary resources in your AWS account.

After successful deployment, it will create a simple stack with AWS Lambda and API Gateway with an endpoint based on API Gateway root resource id e.g.
[https://gbwf8lkzwh.execute-api.us-east-1.amazonaws.com/Prod/hello/](https://gbwf8lkzwh.execute-api.us-east-1.amazonaws.com/Prod/hello/)

![](https://cdn-images-1.medium.com/max/2000/1*sby904FNnv3GKQwEZdiLiw.png)

![](https://cdn-images-1.medium.com/max/2000/1*uIp7Jqu4bZucohUDEL5GKA.png)

### Tracing:

In the .aws-sam/build/template.yaml, tracing is enabled for both the lambda function and the API gateway.

![](https://cdn-images-1.medium.com/max/2000/1*pVHCA_XQH_UEViizoALEpw.png)

You can navigate to AWS X-Ray Service in AWS Console to see end-to-end tracing.

![](https://cdn-images-1.medium.com/max/3172/1*dgSlOYBG3dsRAbeU1jiMPA.png)

**Amazon X-Ray** is a service that helps developers analyze and debug distributed applications, such as those built using a microservices architecture. X-Ray enables you to trace requests as they travel through your application and provides tools to visualize the request data and identify issues with the application.

With X-Ray, you can understand how your application and its underlying resources are performing, identify and troubleshoot the root cause of performance issues, and optimize your application’s performance.

To use X-Ray, you instrument your application code to send data to the X-Ray service. X-Ray then processes and visualizes the data, allowing you to analyze the data and identify issues with your application.

You can use X-Ray with applications running on Amazon EC2, AWS Lambda, and other cloud and on-premises environments.

The above example shows traceability from AWS API Gateway to the AWS Lambda function, but you can’t see traces for the external checkip service.

## Code Instrumentation:

There are two ways to instrument your Go application to send traces to X-Ray:

* [AWS X-Ray SDK for Go](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-go.html) — A set of libraries for generating and sending traces to X-Ray via the [X-Ray daemon](https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon.html).

* [AWS Distro for OpenTelemetry (ADOT) Go](https://docs.aws.amazon.com/xray/latest/devguide/xray-go-opentel-sdk.html) — An AWS distribution that provides a set of open-source libraries for sending correlated metrics and traces to multiple AWS monitoring solutions, including Amazon CloudWatch, AWS X-Ray, and Amazon OpenSearch Service, via the [AWS Distro for OpenTelemetry Collector](https://aws-otel.github.io/docs/getting-started/collector).

This article will focus on AWS X-Ray SDK for code instrumentation.

### Installation:

Install the SDK using the following command (The SDK’s non-testing dependencies will be installed): Use go get to retrieve the SDK to add it to your GOPATH workspace:

    go get github.com/aws/aws-xray-sdk-go

### Configure:

For configuration, you will need to import the package

    import "github.com/aws/aws-xray-sdk-go/xray"

And initialized the x-ray will the following code snippet:

     err := xray.Configure(xray.Config{
      LogLevel: "info",
     })
     if err != nil {
      panic(err)
     }

The Configure function takes a Config struct as an argument and applies the specified configuration values to the X-Ray service.

The LogLevel field in the Config struct specifies the verbosity of the logs that the X-Ray service generates. The value of logLevel can be info or traces

The Configure the function is typically called at the start of the application, before any tracing or instrumentation code is executed, to set the desired log level for the X-Ray service. This allows you to control the amount of log output generated by the service and adjust it to your needs.

### Downstream HTTP Request Instrumentation:

The ctx the argument is the context of the current request. It carries request-scoped values across API boundaries and is cancelable.

The below code snippet will start tracing the HTTP request:

    resp, err := ctxhttp.Get(ctx, xray.Client(nil), DefaultHTTPGetAddress)

The xray.Client function wraps the HTTP client with tracing and performance logging provided by AWS X-Ray. When called with an nil argument, it creates a new HTTP client with default settings and wraps it with the X-Ray middleware.

The ctxhttp package from the Go standard library to make an HTTP GET request to the specified address.

After the deployment, when you open the same URL in the browser, then you will be able to downstream HTTP request tracing as shown below:



![](https://cdn-images-1.medium.com/max/3218/1*LAW1rChQgX6SyxJZxbAzbg.png)

main.go Example:

<iframe src="https://medium.com/media/06b62e02621237617ee016f33db8ea0b" frameborder=0></iframe>



## Source Code

For source code, please refer to the [link](https://github.com/Durgaprasad-Budhwani/aws-lambda-observability-golang-with-xray-sdk)



## Conclusion:

AWS Lambda X-Ray SDK is a tool that enables the observability of serverless applications running on AWS Lambda. It allows developers to trace requests and responses, identify performance bottlenecks, and troubleshoot issues in their serverless applications.

The X-Ray SDK integrates seamlessly with the AWS Lambda runtime and automatically traces Lambda function invocations and their downstream calls. It also enables manual instrumentation of custom logic, allowing developers to get a complete view of their serverless application's request and response flow.

Overall, the AWS Lambda X-Ray SDK is a valuable addition to the toolkit of any developer building serverless applications on AWS. It provides valuable insights into the behaviour and performance of their applications, enabling them to identify and resolve issues quickly and improve the reliability and efficiency of their applications.


