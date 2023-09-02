# DEPLOY
SSL, NginX 등 웹 배포에 필요한 내용 정리 


### 포워딩 

1.내부 IP 확인

hostname -I

2. 포트포워딩 확인

iptables -t nat -L --line-numbers

3.포트포워딩 삭제

iptables -t nat -D PREROUTING [번호]

4.포트포워딩

iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 3000
