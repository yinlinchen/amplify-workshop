```
create-react-app bookapp
cd bookapp && yarn start
```
[Dockerfile](Dockerfile)

```
docker build -t bookapp .
docker run -it -p 8080:80 bookapp
```