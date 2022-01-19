

<h1 align="center">Cataldi.Util.Messaging.AWS</h1>


<p align="center">
  Component to Simplify Using the AWS SDK for Consuming the SQS Messaging Service in .NET Core Projects
</p>



## Important informations
First, This component does not use AWS ***Access Key ID*** and ***Secret Access Key*** to consume the SQS queue on localhost.
To be able to consume it is necessary to be logged first in to AWS through the AWS CLI. <a href="https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html" target="_blank">Installing or updating the latest version of the AWS CLI</a>.


## Settings

### 1º Add as environment variables
The component uses two parameters to access or send a message to the SQS queue, for that add the access environment variables to your queue
In ```launchSettings.json```:
```json
{
  "profiles": {
      "environmentVariables": {
        "AWS_REGION": "us-east-1",
        "AWS_ACCOUNT_ID": "000000000000"
      },
  }
}
```
OR
via code ```startup.cs```:
```csharp
  Environment.SetEnvironmentVariable("AWS_REGION", "us-east-1");
  Environment.SetEnvironmentVariable("AWS_ACCOUNT_ID", "000000000000");
```

### 2º Performing dependency injection
Inject the dependency directly into your **IServiceCollection** of type **AddSingleton** as shown below

``` csharp
  services.AddSingleton<IMessagingAsync, CataldiMessagingSQS>();
```

### 3º Add the reference in the constructor
Get the interface in the constructor
``` csharp
private readonly IMessagingAsync _messagingAsync;

public Constructor(IMessagingAsync messageasync)
  {
      _messagingAsync = messageasync;
  }
```


## How To Use

### Get messages async

This method returns all messages informed in the third and second parameter. The second parameter is the number of messages and the third is the waiting time
The code below return 5 messages with a wait time of 10 seconds
``` csharp
 var messages = _messagingAsync.GetMessages("queue-name", 5, 10).GetAwaiter().GetResult();
 if (messages.Any())
 {
    foreach (var message in messages)
    {
      var queueMessage = message.Body;
      // IMPLEMENT YOUR CODE HERE
    }
 }
```

### Received messages async
This method waits for a message to be sent to the queue, by default the maximum time allowed by AWS is 20 seconds.
```csharp
  var messages = await _messagingAsync.ReceiveMessagesAsync(_initialRegistrationQueue, 20);
```

### Send message
This method sends a message to the specified queue
```csharp
 await _messagingAsync.SendMessage("queue-name", "YOUR-MESSAGE");
 if (messages.Any())
 {
    foreach (var message in messages)
    {
      var queueMessage = message.Body;
      // IMPLEMENT YOUR CODE HERE
    }
 }
```
## Acknowledge message
Method responsible for removing the message from the queue
```csharp
  _messagingAsync.AckMessage("queue-name", message).GetAwaiter().GetResult();
```
## Docs:
SQS usage documentation: <a href="https://docs.aws.amazon.com/sqs/" target="_blank">Amazon Simple Queue Service Documentation</a>
SQS usage documentation: <a href="https://docs.aws.amazon.com/iam/index.html" target="_blank">AWS Identity and Access Management Documentation</a>
## Autor

| [<img src="https://avatars0.githubusercontent.com/u/26310007?v=3&s=115"><br><sub>@dicataldisky</sub>](https://github.com/dicataldisky) |
| :---: |
 
