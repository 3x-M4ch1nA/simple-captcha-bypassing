# CAPTCHA bypassing - POC

## Presentation
Some new CAPTCHAs are only presenting two numbers and asking the user to enter their sum to check if that user is not a robot.<br/>
However the two numbers are simply coded into the html code and therefore very easy to get by parsing the code with regular expressions<br/>

The script presented here was used on a WordPress website where this kind of CAPTCHA was used in the usual "Contact us" form. Therefore the regex are very specific to this website (but can probably work with other WordPress websites).

## Coding of the JMeter script
### Getting both numbers from the page
The JMeter script I made sends a first GET request to the website, parses the html and gets the first and second numbers with a regular expressions that finds their name in the source code: ```data-first_digit``` and ```data-second_digit```.<br/>
A simple regex can get the actual number: ```data\-second\_digit\=\"([0-9]{1,3})\"```.<br/>

### Sending the POST request
Not that we have both numbers, let's move on to the POST request, whihc will send the message.<br/>
For this request, we need the right headers and the right parameters: that's where studying the requests on a browser comes in.<br/>
Once the headers and the parameters are ready, we add the sum of both numbers in the CAPTCHA field:```${__intSum(${firstDig},${secondDig})}```<br/>

## Launching the JMeter script
The user will need JMeter to open and execute the script.
Before launching the JMeter script, the user will need to change:
* the URL (in both requests),
* the parameters of the message to be sent (name, email, message).
Once everything is ready, the user can launch the script and look at the results in the View Results Tree window.

## Story
As POC, I was able to send one message 10 times to one of my friends who has a website with that sort of CAPTCHA.<br/>
It would be interesting to see if one can DoS a server that way...
