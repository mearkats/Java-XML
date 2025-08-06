<p align="center">
  <img src="https://user-images.githubusercontent.com/16869300/191701451-5550afce-b19f-4f8c-9e28-90777dd441e1.png" />
</p>

# Java JRE Download API #

An automated and reliable public API for fetching the latest and historical download links for the Oracle Java JRE.

This project was created to solve the challenge of obtaining direct, stable download links for Java JRE installations, as Oracle's official website is complex and not designed for automated access. This API provides a clean, simple, and always up-to-date solution for developers and IT professionals.

### ⚠️ XML File Deprecation Notice ###
The java.xml file previously maintained in this repository is no longer being updated. Please migrate your tools and scripts to use the live API endpoints below to ensure you always have the most current information.

### How It Works ###
A bot runs on a daily schedule on the mearkats.co.uk backend. It uses a headless browser (Puppeteer) to navigate the official Java download page, handle JavaScript rendering and cookie banners, and then uses a parser (Cheerio) to extract the version information and download links.

If a new version is discovered, the details are stored in a database, which is then exposed by the public API endpoints.

### API Usage ###
Base URL
```
https://www.mearkats.co.uk/api/v1/java/jre
```
### Endpoints ###

#### Get the Latest Version ####
Returns a single JSON object containing the details of the most recent Java release.

```
GET /latest
```

Full URL: ```https://www.mearkats.co.uk/api/v1/java/jre/latest```

#### Get All Historical Versions ####
Returns an array of all collected Java release objects, sorted from newest to oldest.

```
GET /all
```

Full URL: ```https://www.mearkats.co.uk/api/v1/java/jre/all```

### JSON Response Structure ###
The API returns data in the following JSON format. The /latest endpoint returns a single object, while the /all endpoint returns an array of these objects.

```
{
  "id": 59,
  "version_title": "Version 8 Update 461",
  "release_date": "2025-07-15T00:00:00.000Z",
  "links": {
    "windows": {
      "offline_x86": "https://javadl.oracle.com/webapps/download/AutoDL?BundleId=252321_68ce765258164726922591683c51982c",
      "offline_x64": "https://javadl.oracle.com/webapps/download/AutoDL?BundleId=252322_68ce765258164726922591683c51982c"
    },
    "macos": {
      "x64": "https://javadl.oracle.com/webapps/download/AutoDL?BundleId=252315_68ce765258164726922591683c51982c",
      "arm64": "https://javadl.oracle.com/webapps/download/AutoDL?BundleId=252313_68ce765258164726922591683c51982c"
    },
    "linux": {
      "rpm": "https://javadl.oracle.com/webapps/download/AutoDL?BundleId=252309_68ce765258164726922591683c51982c",
      "x64": "https://javadl.oracle.com/webapps/download/AutoDL?BundleId=252312_68ce765258164726922591683c51982c",
      "rpm_x64": "https://javadl.oracle.com/webapps/download/AutoDL?BundleId=252311_68ce765258164726922591683c51982c"
    }
  },
  "created_at": "2025-08-06T12:30:00.000Z"
}
```
### Code Examples ###
Here are some quick examples of how to get the latest Windows 64-bit download link using the API.

#### PowerShell ####
```
$response = Invoke-RestMethod -Uri "https://www.mearkats.co.uk/api/v1/java/jre/latest"
$latestVersion = $response.version_title
$windows64bitLink = $response.links.windows.offline_x64

Write-Host "Latest Version: $latestVersion"
Write-Host "Windows 64-bit Offline Installer: $windows64bitLink"
```

#### JavaScript (Node.js with Axios) ####
```
const axios = require('axios');

async function getLatestJavaLink() {
  try {
    const response = await axios.get('https://www.mearkats.co.uk/api/v1/java/jre/latest');
    const javaInfo = response.data;
    
    const latestVersion = javaInfo.version_title;
    const windows64bitLink = javaInfo.links.windows.offline_x64;

    console.log(`Latest Version: ${latestVersion}`);
    console.log(`Windows 64-bit Offline Installer: ${windows64bitLink}`);
  } catch (error) {
    console.error('Failed to fetch Java info:', error.message);
  }
}
getLatestJavaLink();
```

#### Python (using requests) ####

```
import requests

try:
    response = requests.get("https://www.mearkats.co.uk/api/v1/java/jre/latest")
    response.raise_for_status()  # Raises an exception for bad status codes
    
    java_info = response.json()
    
    latest_version = java_info.get("version_title")
    windows_64bit_link = java_info.get("links", {}).get("windows", {}).get("offline_x64")
    
    print(f"Latest Version: {latest_version}")
    print(f"Windows 64-bit Offline Installer: {windows_64bit_link}")

except requests.exceptions.RequestException as e:
    print(f"Failed to fetch Java info: {e}")
```

## Found This Useful? ##
If this API has helped you, please consider starring the repository on GitHub! ⭐

If you have any questions, run into issues, or would like to request more features, please get in touch via my website's Contact Page.
