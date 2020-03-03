# switch-xci-dl
Download all torrents from switch-xci

## Description
Simple .NET app to download all torrents and bypass the urlshortener

## Result

[![Image from Gyazo](https://i.gyazo.com/56062a094ee2028b592ee60c65aafb92.png)](https://gyazo.com/56062a094ee2028b592ee60c65aafb92)

## Code

```csharp
static void Main(string[] args)
{
var url = "https://switch-xci.com/all-switch-xci-torrents-new-link-updt";
var destination = @"";
var torrentList = new Dictionary<string, string>();

Console.WriteLine($"Starting SwithcXciDL");

using (var wc = new WebClient())
{
	var inner = wc.DownloadString(url);

	var aux = inner.Split(new string[] { "https://ouo.io/st/" }, StringSplitOptions.None).ToList();
	aux.RemoveAt(0);

	foreach (var item in aux)
	{
		try
		{
			var dlUrl = item.Split(new string[] { "https" }, StringSplitOptions.None)[1]
							.Split(new string[] { "\"" }, StringSplitOptions.None)[0];

			var finalUrl = System.Uri.UnescapeDataString(HttpUtility.UrlDecode($"https{dlUrl}"));
			var filename = finalUrl.Split(new string[] { "switchxcitorrent/" }, StringSplitOptions.None)[1];

			torrentList.Add(finalUrl, filename);
		}
		catch
		{
		}
	}

}

Console.WriteLine($"Total torrents found: {torrentList.Count}");
var dlCounter = 0;

foreach (var item in torrentList)
{
	using (var wc = new WebClient())
	{
		try
		{
			string destinationPath = Path.Combine(destination, item.Value);
			wc.DownloadFile(item.Key, destinationPath);
			dlCounter++;
		}
		catch
		{
		}
	}
}

Console.WriteLine($"Total torrents downloaded: {dlCounter}");
Console.ReadKey();
}
```
