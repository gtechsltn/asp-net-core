# ASP.NET Core

# Git

```
echo "# asp-net-core" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin https://github.com/gtechsltn/asp-net-core.git
git push -u origin master
```
## Rate Limiting

```
services.AddMemoryCache();
services.Configure<IpRateLimitOptions>(Configuration.GetSection("IpRateLimiting"));
services.AddSingleton<IIpPolicyStore, MemoryCacheIpPolicyStore>();
services.AddSingleton<IRateLimitCounterStore, MemoryCacheRateLimitCounterStore>();
services.AddSingleton<IRateLimitConfiguration, RateLimitConfiguration>();

// ...

app.UseIpRateLimiting();
```

## Caching responses

```
services.AddResponseCaching()

// ...

app.UseResponseCaching();

```

ProductsController.cs

```
[ApiController]
[Route("api/products")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    [ResponseCache(Duration = 60, VaryByQueryKeys = new[] { "category" })]
    public IActionResult GetProducts(string category)
    {
        // Implementation to retrieve and return products
    }
}

```

### Mastering .NET Web API: Best Practices for Building Powerful APIs

https://blog.devgenius.io/mastering-net-web-api-best-practices-for-building-powerful-apis-da5341f123e9

## Implementing Security in Onion Architecture — ASP.NET Core 8.0

https://levelup.gitconnected.com/implementing-security-in-onion-architecture-asp-net-core-8-0-fe055d4a7a51

### UserProfileController.cs

```
public class UserProfileController : ControllerBase
{
  private readonly ISessionContext _sessionContext;
  private readonly IApplicationService _applicationService;

  public UserProfileController(
    ISessionContext sessionContext
    IApplicationService applicationService)
  {
    this._sessionContext = sessionContext;
    this._applicationService = applicationService;
  }

  // ...

}
```

### ApplicationRepository.cs

```
public class ApplicationRepository: IApplicationRepository
{
  private readonly MyDbContext _dbContext;
  private readonly ISessionContext _sessionContext;

  public ApplicationRepository(
    MyDbContext dbContext, 
    ISessionContext sessionContext)
  {
    this._dbContext = dbContext;
    this._sessionContext = sessionContext;
  }

  // ...

}
```

### SessionContext.cs
```
public class SessionContext : ISessionContext
{
  public readonly ISessionContextAccessor _sessionContextAccessor;
  public readonly UserContext _userContext;

  public SessionContext(ISessionContextAccessor sessionContextAccessor)
  {
    this._sessionContextAccessor = sessionContextAccessor;
  }

  // ...

}
```

### SessionContextAccessor.cs
```
public class SessionContextAccessor : ISessionContextAccessor
{
  private readonly IHttpContextAccessor _httpContextAccessor;
  private readonly ISessionContextService _sessionContextService;

  public SessionContextAccessor(
    IHttpContextAccessor httpContextAccessor,
    ISessionContextService sessionContextService)
  {
    this._httpContextAccessor = httpContextAccessor;
    this._sessionContextService = sessionContextService;
  }

  public string SessionId
  {
    get
    {
      // Implement your own way to persist session information.
      return this._httpContextAccessor
        .HttpContext.Request.Cookies["sessionId"].Value;
    }
  }

  public UserContext GetUserContext()
  {
    var userModel = this._sessionContextService
      .GetUserBySessionId(this.sessionId);
    
    if (userModel != null)
    {
      return new UserContext
      {
        UserId = userModel.UserId,
        FirstName = userModel.FirstName,
        LastName = userModel.LastName,
        EmailAddress = userModel.EmailAddress,
        PhoneNumber = userModel.PhoneNumber
      };
    }
    
    return null;
  }

  // ...

}
```

Above, SessionContextAccessor is again a delegation of a custom SessionContextService service class where we are accessing Session ID through a cookie. Feel free to change the strategy to persist session information yourself whether it be a Claim Provider or any other means.

## Unit Testing

ASP.NET 8 Token Authentication for Web API and React with Integration Testing (Part 1: API)

https://medium.com/c-sharp-progarmming/token-authentication-asp-net-1e7fdfb838bb

ASP.NET 8 Token Authentication for Web API and React with Integration Testing (Part 2: Integration Test)

https://medium.com/@disa2aka/token-authentication-integration-testing-asp-net-1ff49b728faf

ASP.NET 8 Token Authentication for Web API and React with Integration Testing(Part 3: React Frontend)

https://medium.com/@disa2aka/token-authentication-integration-testing-asp-net-32bdab0f9967

## Token Based Authentication Template ASP.NET Core 6.0

https://medium.com/@microclip.lakeesha/net-core-6-token-based-authentication-and-middleware-43a5fc86089a

https://github.com/LakeeshaRCL/Dotnet-6-Web-API/

## Token Based Authentication Template ASP.NET Core 8.0, React, Ant Design, Redux.

https://github.com/andissanayake/UnifiedApp/

## Token Based Authentication in ASP.NET Web API 2.0

https://dotnettutorials.net/lesson/token-based-authentication-web-api/

https://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/

## .NET Web API Unit Testing with xUnit

https://medium.com/@microclip.lakeesha/net-web-api-unit-testing-with-xunit-7da76324cd25

https://github.com/LakeeshaRCL/dotnet-8-web-api-with-unit-tests/

## Pagination in a .NET Web API with EF Core

https://medium.com/@henriquesd/pagination-in-a-net-web-api-with-ef-core-2e6cb032afb7

https://github.com/henriquesd/PaginationDemo
