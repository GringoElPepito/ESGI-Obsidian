```bash
docker run -di --name nginx-kevin -p 80:80 nginx
docker exec -ti nginx-kevin bash
echo 'Kevin est le meilleur' > /usr/share/nginx/html/index.html
```