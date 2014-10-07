## Cookieless Google Analytics

On 26 May 2011 EU introduced a law that requires websites to implement a cookie opt-in mechanism, effectively meaning that the consumer must give his or her consent before cookies or any other form of data is stored in their browser [[1]](https://en.wikipedia.org/wiki/Directive_on_Privacy_and_Electronic_Communications#Cookies). This law especially targets 3rd-party tracking cookies, like those of Google Analytics. As a result, many EU websites nowadays are covered in opt-in or "implied consent" banners, popups and all sorts of nightmares.

However, if your Google Analytics use case only requires visitor tracking for statistical purposes, and not individual targeting, there is an easy and elegant solution to not use cookies and get rid of the nasty popups. This cookieless implementation of Google Analytics was created by [Foture](https://www.foture.net) and is using fingerprinting algorithm created by Valve in [Fingerprintjs](https://github.com/Valve/fingerprintjs).

## What is fingerprinting?

Fingerprinting is a technique, outlined in [the research by Electronic Frontier Foundation][research], of
anonymously identifying a web browser with accuracy of up to 94%. 


A browser is queried for its agent string, screen color depth, language,
installed plugins with supported mime types, timezone offset and other capabilities, 
such as local storage and session storage. Then these values are passed through a hashing function
to produce a fingerprint that gives weak guarantees of uniqueness.

No cookies are stored to identify a browser.

It's worth noting that a mobile share of browsers is much more uniform, so fingerprinting should be used
only as a supplementary identifying mechanism there.

[Read more](http://valve.github.io/blog/2013/07/14/anonymous-browser-fingerprinting/)

## Implementation

Include the `fingerprint.min.js` somewhere on your page (in the `<head>` section, or before the closing `</body>`):
```
<script defer src="fingerprint.min.js"></script>
```

Update your Google Analytics code:
```
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-XXXXXXX-XX', {
    'clientId': new Fingerprint().get(),
    'storage': 'none'
  });
  ga('set', 'anonymizeIp', true);
  ga('send', 'pageview');

</script>
```

In the above code, you need to update the Tracking ID `UA-XXXXXXX-XX`. We instruct GA to not use cookies, by setting `storage` to `none`. Next, we create a unique visitor fingerprint as the `clientId`. Additionally, we are instructing Google to anonymise visitors IP address using the `anonymizeIp` option. Finally, we send a pageview to GA.

If you use this implementation on your site, please let us know! We are looking for people who are willing to test it and compare with their previous GA stats in order to check the accuracy of measurements.

We are successfully using this Cookieless Google Analytics on our websites:
+ [BrightFuture.pl](https://www.brightfuture.pl)
+ [Foture.net](https://www.foture.net)


### Licence

Cookieless implementation of Google Analytics using Fingerprintjs is [MIT][mit] licenced:

Copyright (c) 2014 Foture.net

Original Fingerprintjs code is [MIT][mit] licenced:

Copyright (c) 2013 Valentin Vasilyev

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

[mit]: http://www.opensource.org/licenses/mit-license.php
[research]: https://panopticlick.eff.org/browser-uniqueness.pdf
