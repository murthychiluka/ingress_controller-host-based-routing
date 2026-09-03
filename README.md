self-signed certificate validation:

[root@ip-172-31-71-196 ~]# curl -I -H "Host: app1.example.com" \
http://44.193.48.7
HTTP/1.1 308 Permanent Redirect
Date: Thu, 03 Sep 2026 18:23:09 GMT
Content-Type: text/html
Content-Length: 164
Connection: keep-alive
Location: https://app1.example.com

[root@ip-172-31-71-196 ~]# curl -k --resolve app1.example.com:443:44.193.48.7 \
https://app1.example.com
<html>
  <body>
    <h1>HELLO FROM APP 1</h1>
  </body>
</html>
[root@ip-172-31-71-196 ~]# 
```text
[root@ip-172-31-71-196 ~]# kubectl get secret ingress-tls -n ingress-lab
NAME          TYPE                DATA   AGE
ingress-tls   kubernetes.io/tls   2      25m
```

```text
[root@ip-172-31-71-196 ~]# kubectl describe ingress host-based-ingress -n ingress-lab
Warning: v1 Endpoints is deprecated in v1.33+; use discovery.k8s.io/v1 EndpointSlice
Name:             host-based-ingress
Namespace:        ingress-lab
Address:          a22b97d8fcbc04489b6f1a4e3dd58dc6-431308532.us-east-1.elb.amazonaws.com
Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
TLS:
  ingress-tls terminates app1.example.com,app2.example.com
Rules:
  Host              Path  Backends
  ----              ----  --------
  app1.example.com  
                    /   app1-service:80 (192.168.14.37:80,192.168.52.172:80)
  app2.example.com  
                    /   app2-service:80 (192.168.4.233:80,192.168.50.59:80)
Annotations:        nginx.ingress.kubernetes.io/ssl-redirect: true
Events:
  Type    Reason  Age                From                      Message
  ----    ------  ----               ----                      -------
  Normal  Sync    14m (x3 over 90m)  nginx-ingress-controller  Scheduled for sync
  ```
