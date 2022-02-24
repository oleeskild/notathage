# Mock HttpClient

Created: Apr 28, 2021 11:04 PM
Tags: C#, UnitTest

```csharp
var handlerMock = new Mock<HttpMessageHandler>(MockBehavior.Strict);
handlerMock
   .Protected()
   // Setup the PROTECTED method to mock
   .Setup<Task<HttpResponseMessage>>(
      "SendAsync",
      ItExpr.IsAny<HttpRequestMessage>(),
      ItExpr.IsAny<CancellationToken>()
   )
   // prepare the expected response of the mocked http call
   .ReturnsAsync(new HttpResponseMessage()
   {
      StatusCode = HttpStatusCode.OK,
      Content = new StringContent("[{'id':1,'value':'1'}]"),
   })
   .Verifiable();
 
// use real http client with mocked handler here
var httpClient = new HttpClient(handlerMock.Object)
{
   BaseAddress = new Uri("http://test.com/"),
};
```

