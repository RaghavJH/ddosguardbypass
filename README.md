# DDoSGuardBypass

This is a simple tool that allows scraping of websites protected by ddos-guard.net. It will bypass the browser challenge protection and allow you to access the website content.

## How To Implement

Either download and use the Java class files in your project or import the [.jar](https://github.com/RaghavJH/ddosguardbypass/blob/master/ddosguardbypass.jar) available.

### Example

```java
try {
    // Create bypass object
    DdosGuardBypass ddgbypass = new DdosGuardBypass("https://example.com/");
    // ALTERNATIVELY...
    // You can use proxies with the bypass like so:
    // When using proxies, avoid using https (see known issues on README)
    ddgbypass = new DdosGuardBypass("http://example.com/", "127.0.0.1", 8080);

    // Bypass...
    ddgbypass.bypass();

    try {
        // Get the cookies for your own use
        System.out.println(ddgbypass.getCookiesAsString());
        // Returns HashMap<String, String> (Every Cookie returned from the website)
        ddgbypass.getCookies();
        // Use internal get feature to get a page that has been bypassed
        System.out.println(ddgbypass.get("http://example.com/"));
    } catch (NotYetBypassedException e) {
        e.printStackTrace();
    } catch (NotSameHostException e) {
        e.printStackTrace();
    }
} catch (MalformedURLException e) {
    e.printStackTrace();
}
```
### Important
When entering a URL in the constructor, it is essential that you do not use any paths. For instance if the URL you want to access is "https://www.example.com/aPath?query=aString", then do the following:

```java
try {
    DdosGuardBypass ddgbypass = new DdosGuardBypass("https://www.example.com/");
    ddgbypass.bypass();
    try {
        System.out.println(ddgbypass.get("https://www.example.com/aPath?query=aString"));
    } catch (NotYetBypassedException e) {
        e.printStackTrace();
    } catch (NotSameHostException e) {
        e.printStackTrace();
    }
} catch (MalformedURLException e) {
    e.printStackTrace();
}
```
### Recommendation
I highly recommend you use your own HTTP class to send requests. Simply get the cookies using the ```.getCookiesAsString()``` method to get the cookies and use it in your HTTP class by setting the cookie header. Once you set the cookies, you can send GET / POST requests without any issues.

## Known issues
Doesn't seem to work with proxies when using HTTPS.

## TODO
- Add code documentation and support for POST.
- Fix issue where HTTPS w/ proxies gives errors.
