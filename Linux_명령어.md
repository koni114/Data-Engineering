# Linux 명령어 간단 정리
- DB Connection 개수 보고 싶은 경우
  - `netstat -an | grep tcp | grep 5630 | wc -l`
- Common 접속 후 다른 계정으로 접속
  - `sudo -i`
  - `su - ruser01` 후 ruser01 접속