# Lab: Stored DOM XSS

**Difficulty:** Practitioner

**Link:** https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-stored

## Objective : exploit this vulnerability to call the alert() function.

## Analysis
**Context:** Blog Comment, Elements Panel

**Sink:** The comment is reflecetd in the `user-comments`'s `<p>` tag in the `<section>` tag.

I first tried to check where the comment is reflected after being submitted on the web page. For this I just commented randomly and submitted. Found that it is shown in the comment section, even when I inspected it.

Then tried to test that whether the comment functionality would be storing and executing the payload.
So payload exectuted was this: <script>alert(1)</script>

And, the got this:
![Snap of the payload execution](payload1.jpeg)
![Snap of the payload execution 2](<payload result.jpeg>)

Then got to know that there's some code written in the backend that is encoding the script tag, so `<script>` is been stored rather than executing.

Then payload `<script><script>alert(1)</script>` was executed in the comment functionality, for checking that will backend accept it or not.
![payload 2 execution](<payload1 result.jpeg>)

Here when I inspected I found that the first part of the payload i.e. `<script>` tag was being escaped and stored but then too the other payload part (`<script>alert(1)</script>`) was not executed, so thought maybe it can be so that the backend code was having some code that would escape the functionality of the script tag.
![payload 2 execution 2](<payload result 3.jpeg>)

The other payload that I tried was : `<script><img src=x onerror=alert(1)>`. 
I used an image tag, with an source and onerror attribute. As 'x' is not a valid source so, `src` attribute will return error and when error is returned then `onerror` attribute will execute the `alert(1)`.

![payload 3](<payload 2.jpeg>)
![alert](<alert ss.jpeg>)
![lab solved](<lab solved ss.jpeg>)

By the execution of the 3rd payload through the comment functionality, the lab was solved!