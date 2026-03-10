# ZAP Scanning Report

ZAP by [Checkmarx](https://checkmarx.com/).


## Summary of Alerts

| Risk Level | Number of Alerts |
| --- | --- |
| High | 0 |
| Medium | 2 |
| Low | 0 |
| Informational | 4 |




## Insights

| Level | Reason | Site | Description | Statistic |
| --- | --- | --- | --- | --- |
| Info | Informational | https://scan-dev.xvio.ai | Percentage of responses with status code 2xx | 100 % |
| Info | Informational | https://scan-dev.xvio.ai | Percentage of endpoints with content type application/json | 5 % |
| Info | Informational | https://scan-dev.xvio.ai | Percentage of endpoints with content type image/png | 17 % |
| Info | Informational | https://scan-dev.xvio.ai | Percentage of endpoints with content type image/svg+xml | 5 % |
| Info | Informational | https://scan-dev.xvio.ai | Percentage of endpoints with content type text/css | 5 % |
| Info | Informational | https://scan-dev.xvio.ai | Percentage of endpoints with content type text/html | 11 % |
| Info | Informational | https://scan-dev.xvio.ai | Percentage of endpoints with content type text/javascript | 52 % |
| Info | Informational | https://scan-dev.xvio.ai | Percentage of endpoints with method GET | 100 % |
| Info | Informational | https://scan-dev.xvio.ai | Count of total endpoints | 17    |
| Info | Informational | https://scan-dev.xvio.ai | Percentage of slow responses | 100 % |




## Alerts

| Name | Risk Level | Number of Instances |
| --- | --- | --- |
| CSP: script-src unsafe-inline | Medium | 3 |
| CSP: style-src unsafe-inline | Medium | 3 |
| Modern Web Application | Informational | 3 |
| Re-examine Cache-control Directives | Informational | 4 |
| Retrieved from Cache | Informational | 3 |
| Storable and Cacheable Content | Informational | Systemic |




## Alert Detail



### [ CSP: script-src unsafe-inline ](https://www.zaproxy.org/docs/alerts/10055/)



##### Medium (High)

### Description

Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks. Including (but not limited to) Cross Site Scripting (XSS), and data injection attacks. These attacks are used for everything from data theft to site defacement or distribution of malware. CSP provides a set of standard HTTP headers that allow website owners to declare approved sources of content that browsers should be allowed to load on that page — covered types are JavaScript, CSS, HTML frames, fonts, images and embeddable objects such as Java applets, ActiveX, audio and video files.

* URL: https://scan-dev.xvio.ai
  * Node Name: `https://scan-dev.xvio.ai`
  * Method: `GET`
  * Parameter: `Content-Security-Policy`
  * Attack: ``
  * Evidence: `default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' https://d18e7sz74505kc.cloudfront.net data:; connect-src 'self' blob: data: https://api-dev.xvio.ai wss://appsync-dev.xvio.ai wss://*.appsync-realtime-api.ap-south-1.amazonaws.com https://exonaihub-developer-feedback-412706839343.s3.ap-south-1.amazonaws.com https://*.salesforce.com https://*.force.com https://*.zoho.com https://*.zoho.in https://*.sap.com https://*.amazoncognito.com https://*.microsoft.com https://*.microsoftonline.com; font-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self'`
  * Other Info: `script-src includes unsafe-inline.`
* URL: https://scan-dev.xvio.ai/robots.txt
  * Node Name: `https://scan-dev.xvio.ai/robots.txt`
  * Method: `GET`
  * Parameter: `Content-Security-Policy`
  * Attack: ``
  * Evidence: `default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' https://d18e7sz74505kc.cloudfront.net data:; connect-src 'self' blob: data: https://api-dev.xvio.ai wss://appsync-dev.xvio.ai wss://*.appsync-realtime-api.ap-south-1.amazonaws.com https://exonaihub-developer-feedback-412706839343.s3.ap-south-1.amazonaws.com https://*.salesforce.com https://*.force.com https://*.zoho.com https://*.zoho.in https://*.sap.com https://*.amazoncognito.com https://*.microsoft.com https://*.microsoftonline.com; font-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self'`
  * Other Info: `script-src includes unsafe-inline.`
* URL: https://scan-dev.xvio.ai/sitemap.xml
  * Node Name: `https://scan-dev.xvio.ai/sitemap.xml`
  * Method: `GET`
  * Parameter: `Content-Security-Policy`
  * Attack: ``
  * Evidence: `default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' https://d18e7sz74505kc.cloudfront.net data:; connect-src 'self' blob: data: https://api-dev.xvio.ai wss://appsync-dev.xvio.ai wss://*.appsync-realtime-api.ap-south-1.amazonaws.com https://exonaihub-developer-feedback-412706839343.s3.ap-south-1.amazonaws.com https://*.salesforce.com https://*.force.com https://*.zoho.com https://*.zoho.in https://*.sap.com https://*.amazoncognito.com https://*.microsoft.com https://*.microsoftonline.com; font-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self'`
  * Other Info: `script-src includes unsafe-inline.`


Instances: 3

### Solution

Ensure that your web server, application server, load balancer, etc. is properly configured to set the Content-Security-Policy header.

### Reference


* [ https://www.w3.org/TR/CSP/ ](https://www.w3.org/TR/CSP/)
* [ https://caniuse.com/#search=content+security+policy ](https://caniuse.com/#search=content+security+policy)
* [ https://content-security-policy.com/ ](https://content-security-policy.com/)
* [ https://github.com/HtmlUnit/htmlunit-csp ](https://github.com/HtmlUnit/htmlunit-csp)
* [ https://web.dev/articles/csp#resource-options ](https://web.dev/articles/csp#resource-options)


#### CWE Id: [ 693 ](https://cwe.mitre.org/data/definitions/693.html)


#### WASC Id: 15

#### Source ID: 3

### [ CSP: style-src unsafe-inline ](https://www.zaproxy.org/docs/alerts/10055/)



##### Medium (High)

### Description

Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks. Including (but not limited to) Cross Site Scripting (XSS), and data injection attacks. These attacks are used for everything from data theft to site defacement or distribution of malware. CSP provides a set of standard HTTP headers that allow website owners to declare approved sources of content that browsers should be allowed to load on that page — covered types are JavaScript, CSS, HTML frames, fonts, images and embeddable objects such as Java applets, ActiveX, audio and video files.

* URL: https://scan-dev.xvio.ai
  * Node Name: `https://scan-dev.xvio.ai`
  * Method: `GET`
  * Parameter: `Content-Security-Policy`
  * Attack: ``
  * Evidence: `default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' https://d18e7sz74505kc.cloudfront.net data:; connect-src 'self' blob: data: https://api-dev.xvio.ai wss://appsync-dev.xvio.ai wss://*.appsync-realtime-api.ap-south-1.amazonaws.com https://exonaihub-developer-feedback-412706839343.s3.ap-south-1.amazonaws.com https://*.salesforce.com https://*.force.com https://*.zoho.com https://*.zoho.in https://*.sap.com https://*.amazoncognito.com https://*.microsoft.com https://*.microsoftonline.com; font-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self'`
  * Other Info: `style-src includes unsafe-inline.`
* URL: https://scan-dev.xvio.ai/robots.txt
  * Node Name: `https://scan-dev.xvio.ai/robots.txt`
  * Method: `GET`
  * Parameter: `Content-Security-Policy`
  * Attack: ``
  * Evidence: `default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' https://d18e7sz74505kc.cloudfront.net data:; connect-src 'self' blob: data: https://api-dev.xvio.ai wss://appsync-dev.xvio.ai wss://*.appsync-realtime-api.ap-south-1.amazonaws.com https://exonaihub-developer-feedback-412706839343.s3.ap-south-1.amazonaws.com https://*.salesforce.com https://*.force.com https://*.zoho.com https://*.zoho.in https://*.sap.com https://*.amazoncognito.com https://*.microsoft.com https://*.microsoftonline.com; font-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self'`
  * Other Info: `style-src includes unsafe-inline.`
* URL: https://scan-dev.xvio.ai/sitemap.xml
  * Node Name: `https://scan-dev.xvio.ai/sitemap.xml`
  * Method: `GET`
  * Parameter: `Content-Security-Policy`
  * Attack: ``
  * Evidence: `default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' https://d18e7sz74505kc.cloudfront.net data:; connect-src 'self' blob: data: https://api-dev.xvio.ai wss://appsync-dev.xvio.ai wss://*.appsync-realtime-api.ap-south-1.amazonaws.com https://exonaihub-developer-feedback-412706839343.s3.ap-south-1.amazonaws.com https://*.salesforce.com https://*.force.com https://*.zoho.com https://*.zoho.in https://*.sap.com https://*.amazoncognito.com https://*.microsoft.com https://*.microsoftonline.com; font-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self'`
  * Other Info: `style-src includes unsafe-inline.`


Instances: 3

### Solution

Ensure that your web server, application server, load balancer, etc. is properly configured to set the Content-Security-Policy header.

### Reference


* [ https://www.w3.org/TR/CSP/ ](https://www.w3.org/TR/CSP/)
* [ https://caniuse.com/#search=content+security+policy ](https://caniuse.com/#search=content+security+policy)
* [ https://content-security-policy.com/ ](https://content-security-policy.com/)
* [ https://github.com/HtmlUnit/htmlunit-csp ](https://github.com/HtmlUnit/htmlunit-csp)
* [ https://web.dev/articles/csp#resource-options ](https://web.dev/articles/csp#resource-options)


#### CWE Id: [ 693 ](https://cwe.mitre.org/data/definitions/693.html)


#### WASC Id: 15

#### Source ID: 3

### [ Modern Web Application ](https://www.zaproxy.org/docs/alerts/10109/)



##### Informational (Medium)

### Description

The application appears to be a modern web application. If you need to explore it automatically then the Ajax Spider may well be more effective than the standard one.

* URL: https://scan-dev.xvio.ai
  * Node Name: `https://scan-dev.xvio.ai`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: `<script>
				{
					__sveltekit_1ffupkq = {
						base: ""
					};

					const element = document.currentScript.parentElement;

					Promise.all([
						import("/_app/immutable/entry/start.BwtVambL.js"),
						import("/_app/immutable/entry/app.BlPMhapk.js")
					]).then(([kit, app]) => {
						kit.start(app, element);
					});
				}
			</script>`
  * Other Info: `No links have been found while there are scripts, which is an indication that this is a modern web application.`
* URL: https://scan-dev.xvio.ai/robots.txt
  * Node Name: `https://scan-dev.xvio.ai/robots.txt`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: `<script>
				{
					__sveltekit_1ffupkq = {
						base: ""
					};

					const element = document.currentScript.parentElement;

					Promise.all([
						import("/_app/immutable/entry/start.BwtVambL.js"),
						import("/_app/immutable/entry/app.BlPMhapk.js")
					]).then(([kit, app]) => {
						kit.start(app, element);
					});
				}
			</script>`
  * Other Info: `No links have been found while there are scripts, which is an indication that this is a modern web application.`
* URL: https://scan-dev.xvio.ai/sitemap.xml
  * Node Name: `https://scan-dev.xvio.ai/sitemap.xml`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: `<script>
				{
					__sveltekit_1ffupkq = {
						base: ""
					};

					const element = document.currentScript.parentElement;

					Promise.all([
						import("/_app/immutable/entry/start.BwtVambL.js"),
						import("/_app/immutable/entry/app.BlPMhapk.js")
					]).then(([kit, app]) => {
						kit.start(app, element);
					});
				}
			</script>`
  * Other Info: `No links have been found while there are scripts, which is an indication that this is a modern web application.`


Instances: 3

### Solution

This is an informational alert and so no changes are required.

### Reference




#### Source ID: 3

### [ Re-examine Cache-control Directives ](https://www.zaproxy.org/docs/alerts/10015/)



##### Informational (Low)

### Description

The cache-control header has not been set properly or is missing, allowing the browser and proxies to cache content. For static assets like css, js, or image files this might be intended, however, the resources should be reviewed to ensure that no sensitive content will be cached.

* URL: https://scan-dev.xvio.ai
  * Node Name: `https://scan-dev.xvio.ai`
  * Method: `GET`
  * Parameter: `cache-control`
  * Attack: ``
  * Evidence: ``
  * Other Info: ``
* URL: https://scan-dev.xvio.ai/manifest.json
  * Node Name: `https://scan-dev.xvio.ai/manifest.json`
  * Method: `GET`
  * Parameter: `cache-control`
  * Attack: ``
  * Evidence: ``
  * Other Info: ``
* URL: https://scan-dev.xvio.ai/robots.txt
  * Node Name: `https://scan-dev.xvio.ai/robots.txt`
  * Method: `GET`
  * Parameter: `cache-control`
  * Attack: ``
  * Evidence: ``
  * Other Info: ``
* URL: https://scan-dev.xvio.ai/sitemap.xml
  * Node Name: `https://scan-dev.xvio.ai/sitemap.xml`
  * Method: `GET`
  * Parameter: `cache-control`
  * Attack: ``
  * Evidence: ``
  * Other Info: ``


Instances: 4

### Solution

For secure content, ensure the cache-control HTTP header is set with "no-cache, no-store, must-revalidate". If an asset should be cached consider setting the directives "public, max-age, immutable".

### Reference


* [ https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html#web-content-caching ](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html#web-content-caching)
* [ https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cache-Control ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cache-Control)
* [ https://grayduck.mn/2021/09/13/cache-control-recommendations/ ](https://grayduck.mn/2021/09/13/cache-control-recommendations/)


#### CWE Id: [ 525 ](https://cwe.mitre.org/data/definitions/525.html)


#### WASC Id: 13

#### Source ID: 3

### [ Retrieved from Cache ](https://www.zaproxy.org/docs/alerts/10050/)



##### Informational (Medium)

### Description

The content was retrieved from a shared cache. If the response data is sensitive, personal or user-specific, this may result in sensitive information being leaked. In some cases, this may even result in a user gaining complete control of the session of another user, depending on the configuration of the caching components in use in their environment. This is primarily an issue where caching servers such as "proxy" caches are configured on the local network. This configuration is typically found in corporate or educational environments, for instance.

* URL: https://scan-dev.xvio.ai
  * Node Name: `https://scan-dev.xvio.ai`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: `Hit from cloudfront`
  * Other Info: ``
* URL: https://scan-dev.xvio.ai/robots.txt
  * Node Name: `https://scan-dev.xvio.ai/robots.txt`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: `Age: 1`
  * Other Info: `The presence of the 'Age' header indicates that a HTTP/1.1 compliant caching server is in use.`
* URL: https://scan-dev.xvio.ai/sitemap.xml
  * Node Name: `https://scan-dev.xvio.ai/sitemap.xml`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: `Age: 1`
  * Other Info: `The presence of the 'Age' header indicates that a HTTP/1.1 compliant caching server is in use.`


Instances: 3

### Solution

Validate that the response does not contain sensitive, personal or user-specific information. If it does, consider the use of the following HTTP response headers, to limit, or prevent the content being stored and retrieved from the cache by another user:
Cache-Control: no-cache, no-store, must-revalidate, private
Pragma: no-cache
Expires: 0
This configuration directs both HTTP 1.0 and HTTP 1.1 compliant caching servers to not store the response, and to not retrieve the response (without validation) from the cache, in response to a similar request.

### Reference


* [ https://datatracker.ietf.org/doc/html/rfc7234 ](https://datatracker.ietf.org/doc/html/rfc7234)
* [ https://datatracker.ietf.org/doc/html/rfc7231 ](https://datatracker.ietf.org/doc/html/rfc7231)
* [ https://www.rfc-editor.org/rfc/rfc9110.html ](https://www.rfc-editor.org/rfc/rfc9110.html)


#### CWE Id: [ 525 ](https://cwe.mitre.org/data/definitions/525.html)


#### Source ID: 3

### [ Storable and Cacheable Content ](https://www.zaproxy.org/docs/alerts/10049/)



##### Informational (Medium)

### Description

The response contents are storable by caching components such as proxy servers, and may be retrieved directly from the cache, rather than from the origin server by the caching servers, in response to similar requests from other users. If the response data is sensitive, personal or user-specific, this may result in sensitive information being leaked. In some cases, this may even result in a user gaining complete control of the session of another user, depending on the configuration of the caching components in use in their environment. This is primarily an issue where "shared" caching servers such as "proxy" caches are configured on the local network. This configuration is typically found in corporate or educational environments, for instance.

* URL: https://scan-dev.xvio.ai/_app/immutable/chunks/CWj6FrbW.js
  * Node Name: `https://scan-dev.xvio.ai/_app/immutable/chunks/CWj6FrbW.js`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: ``
  * Other Info: `In the absence of an explicitly specified caching lifetime directive in the response, a liberal lifetime heuristic of 1 year was assumed. This is permitted by rfc7234.`
* URL: https://scan-dev.xvio.ai/_app/immutable/entry/start.BwtVambL.js
  * Node Name: `https://scan-dev.xvio.ai/_app/immutable/entry/start.BwtVambL.js`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: ``
  * Other Info: `In the absence of an explicitly specified caching lifetime directive in the response, a liberal lifetime heuristic of 1 year was assumed. This is permitted by rfc7234.`
* URL: https://scan-dev.xvio.ai/favicon-16.png
  * Node Name: `https://scan-dev.xvio.ai/favicon-16.png`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: ``
  * Other Info: `In the absence of an explicitly specified caching lifetime directive in the response, a liberal lifetime heuristic of 1 year was assumed. This is permitted by rfc7234.`
* URL: https://scan-dev.xvio.ai/favicon.svg
  * Node Name: `https://scan-dev.xvio.ai/favicon.svg`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: ``
  * Other Info: `In the absence of an explicitly specified caching lifetime directive in the response, a liberal lifetime heuristic of 1 year was assumed. This is permitted by rfc7234.`
* URL: https://scan-dev.xvio.ai/fonts/inter.css
  * Node Name: `https://scan-dev.xvio.ai/fonts/inter.css`
  * Method: `GET`
  * Parameter: ``
  * Attack: ``
  * Evidence: ``
  * Other Info: `In the absence of an explicitly specified caching lifetime directive in the response, a liberal lifetime heuristic of 1 year was assumed. This is permitted by rfc7234.`

Instances: Systemic


### Solution

Validate that the response does not contain sensitive, personal or user-specific information. If it does, consider the use of the following HTTP response headers, to limit, or prevent the content being stored and retrieved from the cache by another user:
Cache-Control: no-cache, no-store, must-revalidate, private
Pragma: no-cache
Expires: 0
This configuration directs both HTTP 1.0 and HTTP 1.1 compliant caching servers to not store the response, and to not retrieve the response (without validation) from the cache, in response to a similar request.

### Reference


* [ https://datatracker.ietf.org/doc/html/rfc7234 ](https://datatracker.ietf.org/doc/html/rfc7234)
* [ https://datatracker.ietf.org/doc/html/rfc7231 ](https://datatracker.ietf.org/doc/html/rfc7231)
* [ https://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html ](https://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html)


#### CWE Id: [ 524 ](https://cwe.mitre.org/data/definitions/524.html)


#### WASC Id: 13

#### Source ID: 3


