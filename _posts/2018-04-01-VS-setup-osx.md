---
title:  "Setting up the Visual Studio Development Environment to interact with the Stratis blockchain - OSX"
date:   2018-04-01 16:16:01 -0600
categories: learning bounties
post_importance: 1
bounty: 10
---
This is an unfinished article. Earn {{ page.bounty }} STRAT by completing this article. Edit it [here](https://github.com/StratisDevelopmentFoundation/StratisDevelopmentFoundation.github.io/edit/master/{{ page.path }}).
{: .notice--info}

# Getting started with Stratis Development

This article will show you how to set up Visual Studio so that you can run C# code that interacts with the Stratis blockchain.

## Download and install Visual Studio Community Edition
### Download
- Click [here](https://www.visualstudio.com/vs/community/) to download the latest version of **Visual Studio Community for Mac**.
### Install
1. Open the downloaded Visual Studio .dmg file and follow the instructions.
2. Install the **.Net Core** shown under Platforms during the installation process . You can uncheck the remaining options i.e. macOs, Android, iOS, if you don't require to develop mobile or mac applications. Screenshot below:

![image](https://user-images.githubusercontent.com/2681744/38523840-58b5bc7c-3c6a-11e8-9fb1-f351f053d564.png)

3. After installation completes, Open the Visual Studio from Applications. 
## Create a console project
1. Open Visual Studio and click on **New Project** as shown below:

![image](https://user-images.githubusercontent.com/2681744/38524321-d0de6ae0-3c6b-11e8-8fc1-6e5cae1ecfe7.png)

2. Under **.Net Core** from left navigation menu, Select **App**. 
3. Select **Console Application** under **General** and click **Next** Screenshot below:

![image](https://user-images.githubusercontent.com/2681744/38524480-5a5ef118-3c6c-11e8-928a-100d02641d6a.png)

4. Provide a name to your application as shown below:

![image](https://user-images.githubusercontent.com/2681744/38524636-eea05902-3c6c-11e8-8c4b-f02612a83c31.png)

5. Click **Create** button to create .Net core console application with default template.

## Add the NStratis Nuget package as a resource
You can either use Nuget package manager console or Visual Studio GUI to add **nStratis** package into your solution.
### Using npm
`Install-Package nStratis -Version 4.0.0.52`
### Using Visual Studio GUI
1. From left navigation menu, Select **Dependencies**. From top menu, Select **Project** and click on **Add Nuget Packages**. Screenshot below:

![image](https://user-images.githubusercontent.com/2681744/38525015-50c1991a-3c6e-11e8-903e-91ee12f71b3e.png)

2. Search **Nstratis** in the search bar present in the top right corner of the window.
3. Select **Nstratis** from the search results and click on **Add Package**. Screenshot below:

![image](https://user-images.githubusercontent.com/2681744/38525157-e143b3ec-3c6e-11e8-8693-31a3d952cc50.png)

4. Install the package. **Nstratis successfully added** message appears after package is added to the solution successfully.
## Paste sample code into the program.cs
1.  Add the following _using_ directive at the top of **Program.cs** file:
`using NBitcoin;`
2. Copy and paste the sample code below inside the method `static void Main(string[] args)`.

            Key privateKey = new Key();
            PubKey publicKey = privateKey.PubKey;
            Console.WriteLine($"Public Key: {publicKey}");
            Console.WriteLine($"Public key hash: {publicKey.Hash}");
            Console.WriteLine($"Stratis testnet address: {publicKey.GetAddress(Network.StratisTest)}");
            Console.WriteLine($"Statis mainnet address: {publicKey.GetAddress(Network.StratisMain)}");
            Console.ReadLine();
  
3. Complete code for **Program.cs** file should look similar to screentshot below:
![image](https://user-images.githubusercontent.com/2681744/38525669-ca872600-3c70-11e8-9c94-08ebb0a52e00.png)
## Add a debug mark
Click on the vertical grey bar to add a breakpoint as shown in the screenshot below:

![image](https://user-images.githubusercontent.com/2681744/38525830-67770fac-3c71-11e8-868d-b0aa6e38fb69.png)

## Run and step through the code
1. Run the application in debug mode by pressing **F5** or **CMD+ENTER** key.
2. A blank terminal window will open and stop at the breakpoint set in previous step. Screenshot below:

![image](https://user-images.githubusercontent.com/2681744/38525975-09d68f8e-3c72-11e8-9337-531a6baa9537.png)

3. Press F10 to step over each line of code.  Hover over an specific variable to check the value at run time. Screenshot below:

![image](https://user-images.githubusercontent.com/2681744/38526149-c3a8b2b6-3c72-11e8-8989-2b136585fe7e.png)

4. Press **F5** to continue execution of the application.
5. Check terminal window for the output of the code as shown below:

![image](https://user-images.githubusercontent.com/2681744/38526225-10b45678-3c73-11e8-9498-0a5c7ba9bd52.png)

Congratulations!🎉  . You have now successfully created a new Console project and interacted with the Stratis Blockchain.

Article By @ashirvad-github 
