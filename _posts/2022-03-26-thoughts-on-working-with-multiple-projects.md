---
layout: post
title: Thoughts on working with multiple projects
date:  2022-03-26
---

Recently I found that [caddy](https://caddyserver.com/) is the best way to setup HTTPS for local development(because I was working with barcode scanner witch requires HTTPS)


For those don't know about, you can run localhost with HTTPS:

1. create a Caddyfile

    ```
    localhost, my-project.localhost
    # you need append `tls internal` if you are not using a .localhost domain

    # assume that you are running a server at localhost:3000
    reverse_proxy 127.0.0.1:3000
    ```
2. run caddy

    ```bash
    caddy run
    ```

3. visit the localhost with HTTPS: <https://localhost> or <https://my-project.localhost>


Quite handy right? Then I want to solve a problem with caddy I once encountered - ports conflict. you must know it if you have worked on multiple projects, for example: you are working on a new project, suddenly you need to fix a bug for a old project:

```bash
$ cd old-project
$ rails s
....
`initialize': Address already in use - bind(2) for "127.0.0.1" port 3000 (Errno::EADDRINUSE)
```


Of course, you can change to `rails s -p 3001`, but want to solve it in a different way, we can differentiate projects by subdomain names, for example I wanna old-project.localhost points to old project, new-project.localhost to new project

I can create Caddyfile like this
```
new-project.localhost {
  reverse_proxy unix//tmp/new-project.sock
}

old-project.localhost {
  reverse_proxy unix//tmp/old-project.sock
}

# ... you can create above programmingly

```


Start projects
```
# start two projects, I use `z` to switch to project directories
z new-project
bundle exec puma -e development -b unix:///tmp/new-project.sock

z old-project
bundle exec puma -e development -b unix:///tmp/old-project.sock
```

visit projects
```
curl old-project.localhost # ok
curl new-project.localhost # ok
```

Now, you don't need to think about which project use whith port, you just type the name of the project to visit it

references: 
* <https://github.com/ajeetdsouza/zoxide>
* <https://caddyserver.com/docs/>