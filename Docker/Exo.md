Création d'un conteneur nginx et modification du fichier `/usr/share/nginx/html`.
1ère solution
```bash
docker run -di --name nginx-kevin -p 80:80 nginx
docker exec -ti nginx-kevin bash
echo 'Kevin est le meilleur' > /usr/share/nginx/html/index.html
docker commit nginx-kevin nginx-kevin
```
2ème solution
```bash
docker run -di --name nginx-kevin -p 80:80 nginx
echo 'Kevin est le meilleur' > index.html
docker cp index.html nginx-kevin:/usr/share/nginx/html/index.html
```
