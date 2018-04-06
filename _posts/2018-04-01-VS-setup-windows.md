# Getting started with Stratis Development

This article will show you how to set up Visual Studio so that you can run C# code that interacts with the Stratis blockchain.

## Download and install Visual Studio Community Edition
1. [Download Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)
2. Run the file you have just downloaded and follow the steps in the installer
3. Once you arrive at the 'Workloads' section, scroll down and select '.NET Core cross-platform development' as shown in the screenshot below:
![alt text](/assets/images/workloads.png "VS 2017 Workloads")
4. Complete the installer and start Visual Studio 2017 Community Edition

## Create a console project
1. Create a new console project in Visual Studio, as shown in the screenshot below:
![alt text](/assets/images/new_project_windows.png "VS 2017 New Project")
2. In the New Project window, you need to select Console App (.NET Core)
![alt text](/assets/images/new_project_consoleapp.png "New Console App")
3. Press 'Ok' to create a new .NET Core console application

## Add the NStratis Nuget package as a resource
There is several ways to complete this step. The easiest way to do it, is as follows:
1. Right-click your project to the right and click on 'Manage NuGet packages'
![alt text](/assets/images/manage_packages.png "Manage NuGet packages")
2. In the NuGet window, search for 'NStratis' and press the 'Install' button to the right
![alt text](/assets/images/install_nstratis.png "Install NStratis")

## Paste sample code into the program.cs
Past the following code into the static Main method
```
            Key privateKey = new Key();
            PubKey publicKey = privateKey.PubKey;
            Console.WriteLine($"Public Key: {publicKey}");
            Console.WriteLine($"Public key hash: {publicKey.Hash}");
            Console.WriteLine($"Stratis testnet address: {publicKey.GetAddress(Network.StratisTest)}");
            Console.WriteLine($"Statis mainnet address: {publicKey.GetAddress(Network.StratisMain)}");
            Console.ReadLine();
```

Your Program.cs file should look like this
![alt text](/assets/images/sample_code.png "Sample code")

## Add a debug mark
Add a breakpoint as shown in the screenshot
![alt text](/assets/images/breakpoint.png "Breakpoint")

## Run and step through the code
1. Run your project by pressing F5.
2. A new Console Window will open and the project will 'stop' at the breakpoint you have set in the previous step.
![alt text](/assets/images/run_project.png "Run project")
3. Step over each line of code by pressing F10. You can mouseover properties to see their values
![alt text](/assets/images/step_over.png "Step over")
4. Press F5 again to continue the application
5. Inspect the Console Window to see its output. It should look similar to this:
![alt text](/assets/images/new_keys_addresses.png "New keys and addresses")

Congratulations! You have now successfully created a new Console project and interacted with the Stratis Blockchain.
