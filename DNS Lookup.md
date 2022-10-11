# Tech-Components
Tech Components - which provides details of content
![image](https://user-images.githubusercontent.com/115500959/195164522-c579247d-39eb-45cd-bde4-4d964d81ec53.png)

### What happens when you type a URL into your browser?

The diagram below illustrates the steps.<br>

1. Bob enters a URL into the browser and hits Enter. In this example, the URL is composed of 4 parts:<br>

🔹 scheme - 𝒉𝒕𝒕𝒑://. This tells the browser to send a connection to the server using HTTP.<br>
🔹 domain - 𝒆𝒙𝒂𝒎𝒑𝒍𝒆.𝒄𝒐𝒎. This is the domain name of the site.<br>
🔹 path - 𝒑𝒓𝒐𝒅𝒖𝒄𝒕/𝒆𝒍𝒆𝒄𝒕𝒓𝒊𝒄. It is the path on the server to the requested resource: phone.<br>
🔹 resource - 𝒑𝒉𝒐𝒏𝒆. It is the name of the resource Bob wants to visit.<br>

2. The browser looks up the IP address for the domain with a domain name system (DNS) lookup. To make the lookup process fast,
data is cached at different layers: browser cache, OS cache, local network cache, and ISP cache. <br>

2.1 If the IP address cannot be found at any of the caches, 
the browser goes to DNS servers to do a recursive DNS lookup until the IP address is found (this will be covered in another post). <br>

3. Now that we have the IP address of the server, the browser establishes a TCP connection with the server.<br>

4. The browser sends an HTTP request to the server. The request looks like this:<br>

𝘎𝘌𝘛 /𝘱𝘩𝘰𝘯𝘦 𝘏𝘛𝘛𝘗/1.1<br>
𝘏𝘰𝘴𝘵: 𝘦𝘹𝘢𝘮𝘱𝘭𝘦.𝘤𝘰𝘮<br>

5. The server processes the request and sends back the response. For a successful response (the status code is 200). The HTML response might look like this: <br>

𝘏𝘛𝘛𝘗/1.1 200 𝘖𝘒<br>
𝘋𝘢𝘵𝘦: 𝘚𝘶𝘯, 30 𝘑𝘢𝘯 2022 00:01:01 𝘎𝘔𝘛<br>
𝘚𝘦𝘳𝘷𝘦𝘳: 𝘈𝘱𝘢𝘤𝘩𝘦<br>
𝘊𝘰𝘯𝘵𝘦𝘯𝘵-𝘛𝘺𝘱𝘦: 𝘵𝘦𝘹𝘵/𝘩𝘵𝘮𝘭; 𝘤𝘩𝘢𝘳𝘴𝘦𝘵=𝘶𝘵𝘧-8<br>

<!𝘋𝘖𝘊𝘛𝘠𝘗𝘌 𝘩𝘵𝘮𝘭><br>
<𝘩𝘵𝘮𝘭 𝘭𝘢𝘯𝘨="𝘦𝘯"><br>
𝘏𝘦𝘭𝘭𝘰 𝘸𝘰𝘳𝘭𝘥<br>
</𝘩𝘵𝘮𝘭><br>

6. The browser renders the HTML content.<br>
