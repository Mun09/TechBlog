# PORT 8080 is already used 에러 해결

Windows

netstat -ano | findstr :8080 \
로 PID를 찾아내고\
taskkill /PID \<PID> /F\
를 실행한다.
