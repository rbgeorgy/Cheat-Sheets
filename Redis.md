# Redis

Получить все ключи
```powershell
sudo redis-server --protected-mode no

for key in $(redis-cli -p 6379 keys \*);
  do echo "Key : '$key'" 
     redis-cli -p 6379 GET $key;
done

wsl -t DISTRO-NAME
```
