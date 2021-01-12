# Azure ServiceBus Remoting Transport for Asynco
This remoting transport uses two Azure ServiceBus queues, one for the requests and one for the replies. The one for the requests must be set with Session=false, and on the other hand the one for the replies must be set with Session=true (since it's used to correlate a reply with its request). Just set it up by using

```csharp
services.AddRemoting(options =>
	options.UseServiceBus(opt =>
	{
		// This is used on the sender side, it's the DispatchProxy timeout 
		opt.Timeout = TimeSpan.FromMinutes(1); 
		// This queue must have the setting Session=false
		opt.RequestsQueueName = "<asyncrequestsqueuename>";
		// This queue must have the setting Session=true
		opt.RepliesQueueName = "<asyncrepliesqueuename>";
		// You could specify a ConnectionString
		opt.ConnectionString = "<azureservicebusconnectionstring>",
		// Or, alternatively, you could use these two options for the ManagedIdentity scenario
		// (the Credential setting is optional, and the shown value it's the default one,
		// you could just specify the namespace and let the library do the rest)
		opt.FullyQualifiedNamespace = "<AzureServiceBusFullyQualifiedNamespace>";
		opt.Credential = new DefaultAzureCredential(new DefaultAzureCredentialOptions()
		{
			ExcludeAzureCliCredential = false,
			ExcludeEnvironmentCredential = true,
			ExcludeInteractiveBrowserCredential = true,
			ExcludeManagedIdentityCredential = false,
			ExcludeSharedTokenCacheCredential = true,
			ExcludeVisualStudioCodeCredential = true,
			ExcludeVisualStudioCredential = true
		})
	}))
```
