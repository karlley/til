バックグランドでサーバ起動

```
$ rails s -d
```

port_number で起動しているPIDを調べる > kill

```
$ lsof -i:port_number
$ kill -9 PID
```