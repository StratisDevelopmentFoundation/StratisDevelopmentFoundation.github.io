---
title:  "Adding new functionality to the Stratis Full Node"
date:   2018-04-09 16:16:01 -0600
permalink: /add_feature/
categories: code_sample
post_importance: 1
---
## Adding a new Feature to the Stratis Full Node

In this article we are going to show you how to add a new "Feature" to the Stratis Full Node.

You can think of this as adding new operations that the Stratis Full Node can do. You could, for example, add a way for an external application you are building to create multisig transactions.

The focus of this article is to show you where in the source code to add new functionality. We will do this by adding the simplest functionality we can - when the new feature is initialized, it will log the initialization to the console. We also add an API endpoint, and make it so that when that new endpoint is called, the call is also written to the console.

You can use the same approach to add functionality that is more interesting than simple logging.

Before you start:

Set up your Visual Studio development environment

For the Stratis Full Node code

Open the SFN code in Visual Studio

### Adding a Project

In the solution explorer in Visual Studio, right click on the Features folder and click "Add New Project". Add a ".NET Core" "Class Library" "C#" for the ".NET Core 2.0" framework. Name the Project "Stratis.Bitcoin.Features.TestFeature".

In the "Features" folder in the Solution Explorer, you now see the "TestFeature" listed. Inside that folder you can see a "Class1.cs" file. Rename it to TestFeature.cs (by right clicking on it in Solution Explorer)

### Adding Feature implementation code

Open TestFeature.cs for editing (Double click in Solution Explorer). Paste in the following code.

```cs
using Microsoft.Extensions.Logging; // For logger
using Microsoft.Extensions.DependencyInjection;
using Stratis.Bitcoin.Builder; // For the full node builder
using Stratis.Bitcoin.Builder.Feature; // For adding Feature
using Stratis.Bitcoin.Features.Wallet; // For later adding dependency on the wallet
using Stratis.Bitcoin.Features.TestFeature.Controllers; // For adding an API endpoint

namespace Stratis.Bitcoin.Features.TestFeature
{
    /// <summary>
    /// Test Feature
    /// </summary>
    public class TestFeature : FullNodeFeature //Implements the Full Node Feature class
    {

        /// <summary>Instance logger.</summary>
        private readonly ILogger logger; // Used to log to the log file and console

        public TestFeature(ILoggerFactory loggerFactory) //Dependency injection tells the Full Node how to create a logger object when TestFeature objects are created
        {
            this.logger = loggerFactory.CreateLogger(this.GetType().FullName); //Instantiate the logger
        }

        public override void Initialize()
        {
            this.logger.LogInformation("TestFeature initialized"); // This initialization code runs when the Full Node starts up
        }
    }

    /// <summary>
    /// A class providing extension methods for <see cref="IFullNodeBuilder"/>.
    /// </summary>
    public static class FullNodeBuilderTestFeatureExtension
    {
        public static IFullNodeBuilder UseTestFeature(this IFullNodeBuilder fullNodeBuilder) // We will add a call to this method when we create the Stratis Full Node object (i.e. in StratisD Project) if we want to use this feature
        {
            fullNodeBuilder.ConfigureFeature(features =>
            {
                features
                    .AddFeature<TestFeature>()
                    .DependOn<WalletFeature>() //Dependencies on other features added here. The WalletFeature is not actually needed for logging, but I expect that people taking this tutorial will be adding wallet features, so demonstrating this here is important
                    .FeatureServices(services =>
                    {
                        services.AddSingleton<TestFeatureController>(); // This adds endpoints found in the TestFeatureController to the Full node's API
                    });
            });
            return fullNodeBuilder;
        }
    }
}
```

### Adding API Endpoint Controller

Next we want to add Controller code. Right click on the Project "Stratis.Bitcoin.Features.TestFeature". Click Add - New Folder. Rename the new folder to "Controllers". Right click the "Controllers" folder and click Add New File. Add an "Empty Class" with the name "TestFeatureController".

Paste in the following code in "TestFeatureController.cs":

```cs
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Logging;
using NBitcoin;

namespace Stratis.Bitcoin.Features.TestFeature.Controllers
{
    /// <summary>
    /// Controller providing TestFeature operation
    /// </summary>
    [Route("api/[controller]")] //declares the endpoint
    public class TestFeatureController : Controller
    {
        private readonly ConcurrentChain chain;

        private readonly ILogger logger;

        /// <summary>
        /// Initializes a new instance of the <see cref="TestFeatureController"/> class.
        /// </summary>
        /// <param name="chain">The chain.</param>
        public TestFeatureController(ILoggerFactory loggerFactory, ConcurrentChain chain) //dependency injection manages creating the requested parameters for the constructor
        {
            this.logger = loggerFactory.CreateLogger(this.GetType().FullName);
            this.chain = chain;
        }

        /// <summary>
        /// Logs the request and returns the block height
        /// </summary>
        /// <returns>Blog height</returns>
        /// <example>/api/testfeature/test1</example>
        [HttpGet]
        [Route("test1")] // the endpoint name
        public IActionResult Test1()
        {
            this.logger.LogInformation("TestFeature endpoint hit."); //Logs the endpoint was called
            return this.Json(chain.ToString()); //returns the block height as a string
        }

    }
}
```

### Adding required dependencies to the new Project

The code we've added won't currently compile because the Project we made does have the dependencies added.

Right click on "Dependencies" under "Stratis.Bitcoin.Features.TestFeature". Click "Edit References" and check the boxes for "NBitcoin", "Stratis.Bitcoin", "Stratis.Bitcoin.Features.Wallet"

Click "Build All" in Visual Studio. The build should succeed.

### Add the new feature to StratisD

StratisD is an example Stratis Full Node for the Stratis Main net. We are going to make it so that it uses the TestFeature we have created.

Go to the Stratis.StratisD Project in the solution explorer and expand it. Double click on Program.cs. Add "using Stratis.Bitcoin.Features.TestFeature;" to the using statements at the top of the file. Add ".UseTestFeature()" after the ".UseWallet()" line.

The file should now look like this:

```cs
using System;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.CodeAnalysis.CSharp;
using NBitcoin;
using NBitcoin.Protocol;
using Stratis.Bitcoin.Builder;
using Stratis.Bitcoin.Configuration;
using Stratis.Bitcoin.Features.Api;
using Stratis.Bitcoin.Features.BlockStore;
using Stratis.Bitcoin.Features.Consensus;
using Stratis.Bitcoin.Features.MemoryPool;
using Stratis.Bitcoin.Features.Miner;
using Stratis.Bitcoin.Features.RPC;
using Stratis.Bitcoin.Features.TestFeature;
using Stratis.Bitcoin.Features.Wallet;
using Stratis.Bitcoin.Utilities;

namespace Stratis.StratisD
{
    public class Program
    {
        public static void Main(string[] args)
        {
            MainAsync(args).Wait();
        }

        public static async Task MainAsync(string[] args)
        {
            try
            {
                Network network = args.Contains("-testnet") ? Network.StratisTest : Network.StratisMain;
                NodeSettings nodeSettings = new NodeSettings(network, ProtocolVersion.ALT_PROTOCOL_VERSION, args:args, loadConfiguration:false);


                // NOTES: running BTC and STRAT side by side is not possible yet as the flags for serialization are static
                var node = new FullNodeBuilder()
                    .UseNodeSettings(nodeSettings)
                    .UsePosConsensus()
                    .UseBlockStore()
                    .UseMempool()
                    .UseWallet()
                    .UseTestFeature()
                    .AddPowPosMining()
                    .UseApi()
                    .AddRPC()
                    .Build();

                if (node != null)
                    await node.RunAsync();
            }
            catch (Exception ex)
            {
                Console.WriteLine("There was a problem initializing the node. Details: '{0}'", ex.Message);
            }
        }
    }
}
```

### Add dependencies to StratisD

Right click on Dependencies under "Stratis.StratisD" and click "Edit References".

Enable the checkbox for "Stratis.Bitcoin.Features.TestFeature"

Click "Build All" in Visual Studio. The build should succeed.

### Run the full node

Right click on the "Stratis.StratisD" project and click "Run Item". Watch the console window. Among the startup messages you will see the "TestFeature initialized" message that we added. Leave the node running while we do the next step.

### Test the new endpoint

Open up swagger in a browser window (typically http://localhost:37221/swagger/ for stratis main net). Expand the "TestFeature" and click "Try it out" to hit the endpoint. We receive in response the block height, as expected.

In the console window for StratisD, we see the message we added to our controller, "TestFeature endpoint hit."

## Conclusion

We've learned here how we can add our own custom features to the Stratis full node. You will notice that there are lots of other feature projects already set up - We can learn a lot by inspecting these. We can make our own modifications to them, or we can add new features of our own. In particular, Take a look at "Stratis.Bitcoin.Features.Wallet". The controller in this Project shows a lot about how to make transactions and inspect wallets using the Stratis Full Node.

The source code for the changes made during this article can be found [here on github](https://github.com/StratisDevelopmentFoundation/StratisBitcoinFullNode/commit/5ff2c4de5105720f9ab1455a84521f6c6e2df992).
