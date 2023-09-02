# DEPLOY
SSL, NginX 등 웹 배포에 필요한 내용 정리 


### 포워딩 

1.내부 IP 확인
``````````
hostname -I
``````````
2.포트포워딩 확인
``````````
iptables -t nat -L --line-numbers
``````````
3.포트포워딩 삭제
``````````
iptables -t nat -D PREROUTING [번호]
``````````
4.포트포워딩
``````````
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 3000
``````````
### SSL 

### NginX
``````````
sudo apt-get install nginx
nginx -v

cd /etc/nginx/sites-enabled
sudo vi domain.conf

(아래 작성)

sudo systemctl restart nginx
sudo systemctl status nginx

``````````

1. SSL WAS에 안하고 로드밸런싱으로 설정
2. 실제 WAS는 http로 띄우고 80, 443에 대한 분기

``````````
server { 
  listen 80; # 80포트로 받을 때
  server_name dailylifestory.co.kr; # 도메인주소
  return 301 https://dailylifestory.co.kr/home;

}

server {
        listen 443 ssl http2;
        # 내 도메인 주소
        server_name dailylifestory.co.kr;

        ssl_certificate /etc/letsencrypt/live/dailylifestory.co.kr/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/dailylifestory.co.kr/privkey.pem;

        location / {
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header Host $http_host;
          proxy_set_header X-NginX-Proxy true;
          proxy_set_header X-Forwarded-Proto $scheme;
          # 서비스 되고 있는 Express App 포트

          proxy_pass http://127.0.0.1:3000;
          proxy_redirect off;

    }
}
``````````
