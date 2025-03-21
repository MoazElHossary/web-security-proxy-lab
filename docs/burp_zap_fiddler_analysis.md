# Steps

## Burp Suite

### a) Try to change the hostname to www.google.com.
![image](https://github.com/user-attachments/assets/6dcd1168-72eb-4115-881b-d06af6a2e169)  
**Figure 1:** Changing Hostname in Burp Suite in the intercept tab.  

In the screenshot above, in the intercept tab the **Host** header is changed from our loopback address to 'www.google.com' to redirect the request to a different server.

![image](https://github.com/user-attachments/assets/9c04626c-ebc0-4ae6-b183-553185a23267)  
**Figure 2:** Modified hostname HTTP Request and Response in Burp Suite.

After sending the modified request, we move to the **HTTP history tab** where we can see our modified request with its response. We can see that the request was still accepted, responding with a **200 OK** HTTP code message. This is because the host header is usually for virtual hosting, since the request was still sent to our loopback, the server still processed it despite changing the host header to google.
However this still highlights a common vulnerability as improperly configured servers can be subject to Host Header injection attack.

---

### Let try using the repeater 
![image](https://github.com/user-attachments/assets/433e9bc9-0162-45f2-ad1c-7732ef8ae8e3)  
**Figure 3:** Changing target from loopback to Google in Repeater. 

To effectively redirect the request to Google’s web server, we changed the target at the top from our **loopback to 'www.google.com'**, this should successfully forward the traffic to google instead of our loopback address.

![image](https://github.com/user-attachments/assets/69201d2a-38af-4bd4-b6ac-d64cafa12054)  
**Figure 4:** 404 Not Found  

The response of our modified request returned a **404 Not Found Erro**r, this is because our website is in a local directory and Google’s server does not recognize our /final directory since it's there. Therefore resulting in a 404 Not Found error.

---

## B) Try GET requests with additional nonstandard headers and different HTTP versions. Why do some attackers prefer to use HTTP/1.0?

![image](https://github.com/user-attachments/assets/3fa6e244-b6f3-42b8-a663-4f4cc4159813)  
**Figure 5:** Enable HTTP/1.0 in request and response. 

Both requests to the server and responses to the client are enabled to have HTTP/1.0. 

**Why do some attackers prefer to use HTTP/1.0?**
Since HTTP/1.0 lacks modern security features such as **persistent connections (keep-alive)**, attackers often prefer to use HTTP/1.0, which is much easier to manipulate and exploit server behaviors.

### Reasons why attacker prefer HTTP/1:
- Lack of header restriction
- No persistent connections
- Proxy server manipulation

![image](https://github.com/user-attachments/assets/f089c19d-6d04-4ccf-9a19-5e02ef745aaf)  
**Figure 6:** Adding an extra header using HTTP/1.0.

![image](https://github.com/user-attachments/assets/695170fe-572e-4245-8d35-dd3ec5bf0dce)  
**Figure 7:** Request sent successfully. 

In the screenshot above we can see our modified header with an additional custom header was **accepted** since we changed the HTTP version from **HTTP/1.1 to HTTP/1.0**. The server responded with a **200 OK** meaning the request was successful, meaning the website accepts HTTP/1.0 making it prone to many attacks such as downgrade attack and header injection.

---

## c) When requesting the form page, try a GET request, but change the POST to GET using a proxy

![image](https://github.com/user-attachments/assets/0e17b13a-4444-4cca-a435-06838a225aae)  
**Figure 8:** Sign in using POST. 

The screenshot above demonstrates a **login attempt using the POST method** from our website. We can ensure we logged in using POST from our credentials taken within the **body of the HTTP request**, which is what mainly differentiates between **GET and POST**, also in the header it is mentioned that the request is sent as POST.

![image](https://github.com/user-attachments/assets/8031f096-5c42-4c9b-9478-bbc3a7f22592)  
**Figure 9:** Right-click request and click "Change Method".  

![image](https://github.com/user-attachments/assets/27619bef-6011-407f-971c-45fd37d88a01)  
**Figure 10:** Send and analyze response.  

The method is changed from **POST to GET** using the **“Change Request Method”** option in Burp Suite, and it sends back a response of **200 OK** meaning our modified request was accepted. Moreover this affected our website by redirecting us to the **POST login page** mentioning that we logged in using **GET and not POST**, which ensures we succesfully manipulated the Request.

---

# ZAP

## a. Try to control the usage of this Cookie by intercepting the request and adding additional headers such as HTTPOnly, Secure, Expiration Date, and Path.
![image](https://github.com/user-attachments/assets/6d5dd245-85c2-43f3-bf2c-8a4c40ebe05a)  
**Figure 11:** Configuring ZAP.  
Go to Network>Local server Proxies, then set the address to localhost and the port to 8080

![image](https://github.com/user-attachments/assets/11887eb0-d7bb-4766-8126-e6f295ae782e)  
**Figure 12:** Enable HUD Mode.  


![image](https://github.com/user-attachments/assets/a308261e-5b02-447d-b89d-569244aadee8)  
**Figure 13:** Turn break on to break on requests.


![image](https://github.com/user-attachments/assets/31fe0c12-da52-4205-902b-76747a41515f)  
**Figure 14:** Adding cookies in the response tab.  
When refrshing, Zap will present the HTTP request and response. In this stage we do navigate to response and add our cookie with the params like user, data, age and path, like the screenshot above.

![image](https://github.com/user-attachments/assets/5a495760-3ff5-4dd6-8490-ea461bb9a0ba)  
**Figure 15:** Press F12 and navigate to the storage tab.  
To verify the cookie has been added, it should be present in the Storage tab in the Developer Tools. 


- Lets delete that cookie and try to add another but with httponly, secure flags and same site.

![image](https://github.com/user-attachments/assets/c99f2fa9-f6f9-4a94-a5e5-0123e97ac11d)  
**Figure 16:** Adding another cookie with flags.

![image](https://github.com/user-attachments/assets/a9f6cfbb-a640-4f11-ab34-65d59f67db7e)  
**Figure 17:** Flags are successfully set.  
The configured parameters in our cookie have been successfully added as shown in the storage section.

![image](https://github.com/user-attachments/assets/ae7abca7-818e-4fc7-a969-f8d21c2172b3)  
**Figure 18:** Making a new request to check if the cookie is present.  
Using ZAP to recording a fresh request demonstrates how cookies move from one request to another. Several consecutive requests maintain the presence of stored cookies because one of the previously set cookies successfully persists.

---

## b. Try to set the Expiration date of the cookie to an older date than the current date.  

![image](https://github.com/user-attachments/assets/0fe29623-541f-4960-ad23-741800b05743)  
**Figure 19:** Add outdated cookie.  

![image](https://github.com/user-attachments/assets/c5c4ebf0-66c1-4437-b43d-a1ed62ed6d2c)  
**Figure 20:** The outdated cookie gets deleted. 
When an existing cookie must be updated (which happens when an outdated cookie is added), the cookie is immediately deleted and any older cookies.

---


## HTTP  
An unsecured website makes Fiddler display all HTTP communication data as plain text through headers and syntax and text views. HTTP communication remains entirely devoid of encryption because of which it becomes susceptible to interception attacks.  
### Requesting an HTTP Website  
![image](https://github.com/user-attachments/assets/1349c81d-1844-4acd-b697-107e3f76c202)  
**Figure 23:** Requesting an HTTP website  

### HTTP Header Response  
![image](https://github.com/user-attachments/assets/c564d48b-1c02-4619-a7d2-e9cff19ac83b)  
**Figure 24:** HTTP Header Response in Plaintext 

### Syntax View of HTTP Website  
![image](https://github.com/user-attachments/assets/10f1d23a-be95-4d90-9114-392e2fadb820)  
**Figure 25:** SyntaxView of HTTP Website in Plaintext  

### RAW HTTP Website  
![image](https://github.com/user-attachments/assets/1067fa71-9ed0-414d-ba1b-83cd698ff412)  
**Figure 26:** RAW HTTP Website in Plaintext  

---

## HTTPS
- The website protects sensitive data using TLS encryption.
- The gateway functionality of Fiddler lacks decryption ability unless users provide the   
appropriate certificate to decrypt the passing traffic.  
### Requesting an HTTPS Website  
![image](https://github.com/user-attachments/assets/81a8498b-86ae-445d-a1b4-ad36a7df1488)  
**Figure 27:** Requesting an HTTPS website  

### Header of HTTPS Website  
![image](https://github.com/user-attachments/assets/2cca9c37-e31a-4d8e-aca8-629c44d9a9c4)  
**Figure 28:** HTTPS Header Response(Encrypted)  

### Syntax View of HTTPS Website  
![image](https://github.com/user-attachments/assets/5619a334-26cd-4e1d-9c8b-3ed3532b2d13)  
**Figure 29:** SyntaxView of HTTPS Website(Encrypted)   

### RAW HTTPS Website  
![image](https://github.com/user-attachments/assets/c7a630d9-607b-4ca5-ad5a-30dfa2171847)  
**Figure 30:** RAW HTTPS Website(Encrypted)    
