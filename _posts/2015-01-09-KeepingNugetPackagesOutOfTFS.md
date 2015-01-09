---
layout: post
title: "Prevent NuGet from storing packages in TFS"
published: true
---

## Prevent NuGet from storing packages in TFS

You can prevent NuGet from adding it's packages into TFS. Starting from TFS 2013, on build it will automatically restore the packages based on the packages\repositories.config.
This will keep the branches under source control clean and prevents old packages from cluttering TFS.

First add a new folder .nuget at the root of your solution.
In the folder create a file called NuGet.config with the following content:

{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <solution>
	<add key="disableSourceControlIntegration" value="true" />
  </solution>
</configuration>
{% endhighlight %} 

You can check the .nuget folder in to TFS.

When you add NuGet packages to the solution now, they should not appear in your pending changes.
