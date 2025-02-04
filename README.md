# Sending cURL POST Requests

[![Promo](https://github.com/luminati-io/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.com/) 

[cURL](https://curl.se/) is a command line tool for transferring data, most commonly by sending HTTP requests. You can use it to send requests to REST APIs and scrape websites. This guide explains what a cURL POST request is, how to send it, and what command line arguments you can use while doing that.

- [What Is a POST Request](#what-is-a-post-request)
- [Installing cURL](#installing-curl)
- [Making a POST Request](#making-a-post-request)
  - [Specifying the POST Method](#specifying-the-post-method)
  - [Setting the Content-Type](#setting-the-content-type)
  - [Sending Data](#sending-data)
  - [Uploading Files](#uploading-files)
  - [Sending Credentials](#sending-credentials)
- [Important cURL Arguments](#important-curl-arguments)
- [Conclusion](#conclusion)

## What Is a POST Request

A POST request is a widely used HTTP method for transmitting data to a server.

Unlike GET requests, where data is appended to the URL, a POST request includes the transmitted data in the request body. This approach enhances privacy and allows for the transfer of more data, bypassing the URL length limitations imposed by browsers.

POST requests are commonly utilized for submitting forms, uploading files, and sending JSON data to APIs. Unlike GET requests, they are generally not cached, and the data remains hidden from browser history since it is contained within the request body.

## Installing cURL

Before you trying using cURL, let's verify if it's installed on your system. Run this command in the terminal:

```bash
curl --version
```

If you see an error message saying that the command is not available, you need to install cURL.

On Windows 11 with WinGet:

```bash
winget install curl.curl
```

On Windows with Chocolatey:

```bash
choco install curl
```

On macOS with Homebrew:

```bash
brew install curl
```

On Ubuntu/Debian Linux:

```bash
apt-get install curl
```

On Red Hat Enterprise Linux, CentOS, and Fedora Linux:

```bash
yum install curl
```

If you run another flavor of Linux, please use its native package manager (e.g. `zypper` on openSUSE or `pacman` on Arch).

## Making a POST Request

### Specifying the POST Method

You need to specify the HTTP method you want to perform when making HTTP requests. The most common methods are GET, POST, PUT, and DELETE. To specify the method, use the `-X` (or `--request`) command line flag.

For example, to send a POST request to `https://httpbin.org/anything`, do this:

```bash
curl -X POST https://httpbin.org/anything
```

httpbin.org echoes the request body and headers in the response. Your response should look like this:

```json
{
  "args": {},
  "data": "",
  "files": {},
  "form": {},
  "headers": {
    "Accept": "*/*",
    "Host": "httpbin.org",
    "User-Agent": "curl/8.1.2",
    "X-Amzn-Trace-Id": "Root=1-11111111-111111111111111111111111"
  },
  "json": null,
  "method": "POST",
  "origin": "0.0.0.0",
  "url": "https://httpbin.org/anything"
}
```

### Setting the Content-Type

When you make a POST request, define the content type in the request body to ensure the server correctly interprets the data. This is done by setting the `Content-Type` header with the appropriate MIME type.

For example, if you're sending JSON in the request body, you need to tell the web server to expect JSON content by setting the `Content-Type` header to `application/json`. To do that, use the `-H` (or `--header`) flag:

```bash
curl -X POST -H 'Content-Type: application/json' -d '{}' https://httpbin.org/anything
```

### Sending Data

The `-d` (or `--data`) flag lets you specify the data to send in the request body. You can use the flag to pass any string of values surrounded by quotations, like this:

```bash
curl -X POST -H 'Content-Type: application/json' -d '{
    "FirstName": "Joe", 
    "LastName": "Soap" 
}' https://httpbin.org/anything
```

If you have a file with data that you want cURL to send in the body, specify the file name prefixed with the `@` character:

```bash
curl -X POST -H 'Content-Type: application/json' -d @body.json https://httpbin.org/anything
```

#### Sending JSON Data

When sending JSON data, set both the `Content-Type` and `Accept` headers to `application/json`:

```bash
curl -X POST -H 'Content-Type: application/json' -H 'Accept: application/json' -d '{
    "FirstName": "Joe",
    "LastName": "Soap"
}' https://httpbin.org/anything
```

cURL offers a `--json` flag (available since version 7.82) that sets these headers automatically:

```bash
curl -X POST --json '{
    "FirstName": "Joe",
    "LastName": "Soap"
}' https://httpbin.org/anything
```

#### Sending XML Data

To send XML data, set the appropriate content headers and provide XML in the request body:

```bash
curl -X POST -H 'Content-Type: application/xml' -H 'Accept: application/xml' -d '<Person>
    <FirstName>Joe</FirstName>
    <LastName>Soap</LastName>
</Person>' https://httpbin.org/anything
```

#### Sending FormData

For form submissions, use the `-F` (or `--form`) flag with field name and value pairs:

```bash
curl -X POST -F FirstName=Joe -F LastName=Soap https://httpbin.org/anything
```

### Uploading Files

To upload files using FormData, prefix the file path with `@`:

```bash
curl -X POST -F File=@Contract.pdf https://httpbin.org/anything
```

### Sending Credentials

For basic authentication, use the `-u` (or `--user`) flag with username and password:

```bash
curl -X POST -u 'admin:password123' --json '{
    "FirstName": "Joe",
    "LastName": "Soap"
}' https://httpbin.org/anything
```

## Important cURL Arguments

| Argument | Full Option | Short Description |
|----------|-------------|-------------------|
| -X | --request | Specify HTTP method used |
| -d | --data | Send request body data |
| -F | --form | Send form-based data |
| -H | --header | Set request header fields |
| -i | --include | Include response headers output |
| -v | --verbose | Show extra debug details |
| -L | --location | Follow redirects automatically |
| -o | --output | Write response to file |
| -O | --remote-name | Save as remote filename |
| -C | --continue-at | Resume transfer from offset |
| -u | --user | Set basic auth credentials |
| -x | --proxy | Use specified proxy server |
| -k | --insecure | Skip SSL certificate checks |
| -I | --head | Fetch headers only (HEAD) |
| -b | --cookie | Send stored cookies data |
| -c | --cookie-jar | Write cookies to file |
| -e | --referer | Set Referer header |
| -A | --user-agent | Set custom User-Agent |
| -m | --max-time | Maximum request time limit |
| --max-filesize | (no short form) | Limit downloaded file size |
| -z | --time-cond | Download if modified since |
| --compressed | (no short form) | Request compressed response |
| --data-urlencode | (no short form) | URL-encode data fields |
| --interface | (no short form) | Bind to specific interface |
| -w | --write-out | Print custom response info |
| -r | --range | Request specific byte range |
| -s | --silent | Suppress progress output |
| -f | --fail | Fail silently on errors |
| -g | --globoff | Disable bracket expansions |
| --http1.0 | (no short form) | Use HTTP/1.0 protocol |
| --http1.1 | (no short form) | Use HTTP/1.1 protocol |
| --http2 | (no short form) | Use HTTP/2 protocol |
| --http3 | (no short form) | Use HTTP/3 protocol |
| -Z | --parallel | Send requests simultaneously |
| --proxy-user | (no short form) | Proxy authentication credentials |
| --digest | (no short form) | Use HTTP Digest auth |
| --ntlm | (no short form) | Use HTTP NTLM auth |
| --negotiate | (no short form) | Use SPNEGO/Kerberos auth |

## Conclusion

cURL offers many additional features worth exploring, such as using variables, sending cookies, and working with proxies. To avoid exposing your public IP address when making automated HTTP requests, consider using [Bright Data's proxy servers](https://brightdata.com/proxy-types) to mask your IP address. Create a free Bright Data account today to test our proxies and scraping solutions!