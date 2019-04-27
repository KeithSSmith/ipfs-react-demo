This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

# React Webhosting on IPFS
This project followed the following guide to get a simple React Website hosted on IPFS: https://medium.com/elbstack/decentralized-hosting-of-a-static-react-app-with-ipfs-aae11b860f5e

## IPFS on Docker
```bash
export ipfs_staging=~/ipfs/staging
export ipfs_data=~/ipfs/data

docker run -d --name ipfs_host \
 -v $ipfs_staging:/export \
 -v $ipfs_data:/data/ipfs \
 -p 4001:4001 \
 -p 127.0.0.1:8080:8080 \
 -p 127.0.0.1:5001:5001 \
 ipfs/go-ipfs:latest

docker exec ipfs_host ipfs swarm peers
```

### Firewall Port Forwarding
In order to talk to other IPFS nodes it will be required to port forward the port you choose above that maps to the Docker port 4001. In our case it is the same so 4001 is port forwarded to ensure it can communicate with the rest of the IPFS network.

### IPFS Bash Function
Setup a local alias to call the Docker container like it was a local command.
```bash
echo 'function ipfs() {
  docker exec ipfs_host ipfs "$@"
}' >> ~/.bash_aliases

. ~/.bashrc
ipfs ls QmZ5eFjjYFA7WTZhFQzko9rm9K2hUqsrB8Z81XJjtde4x3
```

## Create React App
```bash
npx create-react-app ipfs-react-demo
cd ipfs-react-demo
yarn start
```
At this point you can edit App.js to your hearts content, this repo has kept it simple and only added words at the top of the page. Once complete you will need to build the react application.

### Edit package.json
Add `"homepage": "./",`

### Build Project
```bash
yarn build
```

## Synchronize Files and Commit to IPFS
The second part of this section applies to everyone while the first section applies to my situation which is developing on a remote server and synchronizing files to node hosting the IPFS Docker instance.

### PyCharm Deployment
File -> Settings -> Build, Execution, Deployment -> Deployment

Configure SFTP connection with remote host.
Map the local path and the remote path.

Under options, set the files to ignore and change the upload preferences to Always or On Save.

### Commit to IPFS
Once the files and directories have been synchronized it is now time to add the directory to IPFS; NOTE: it is always recommended to uplaod a directory for websites hosted on IPFS even if it is just and index.html file.

```bash
cp -r <base_directory>/ipfs-react-demo/build $ipfs_staging
ipfs add -r /export/build
```

### Retrieve IPFS Hash
Once the directory has been added to IPFS it will generate a hash that looks like the following.
```
$ ipfs add -r /export/build
added QmcPdq8ocoPACiSR6u3WWFubDEsre6a2pG7tBz9dru4AU8 build/asset-manifest.json
 4.59 KiB / ? added QmcFc6EPhavNSfdjG8byaxxV6KtHZvnDwYXLHvyJQPp3uN build/favicon.ico
added Qmc1jd9uf5DmSqRcjsq5iUJ5NswcPE2jCYbzLJ8fhbbpmd build/index.html
 7.53 KiB / 478.59 KiB    1.57%added QmfKqCqGsesAk1JhovJCQajk8awxuqjDPc8QnHa39oDx6g build/manifest.json
 7.53 KiB / 478.59 KiB    1.57%added QmZHHbsBeUVuNPhAniFBQGnssWbKVoCoS6YZi6rVGv8CQY build/precache-manifest.c75e6fbb8aeee470ab7435fbd601c351.js
 9.67 KiB / 478.59 KiB    2.02%added QmPFhRY5UiP77Jn6P87KjV5RKAXM2KzePf29vRVR35C25d build/service-worker.js
 9.67 KiB / 478.59 KiB    2.02%added QmUs7uyggmSQxBrEGUiYY4khG99nxr3wvtJG6acrdwm6Yq build/static/css/main.584f321a.chunk.css
added QmSWcsYLnHUxV4YStPcdJR7u83xpp9iiy3yfv8gYqGbURK build/static/css/main.584f321a.chunk.css.map
 127.09 KiB / 478.59 KiB   26.56%added QmVMiAjUhVZbn4PDymNu8QrK3NsDgJRRvZA5q7UvV5rFFV build/static/js/2.b41502e9.chunk.js
 459.60 KiB / 478.59 KiB   96.03%added QmP8p6ExszCZ3Z3aFbJ6kEeCnPDPgQ8R4EwCepVCPby3uv build/static/js/2.b41502e9.chunk.js.map
 459.60 KiB / 478.59 KiB   96.03%added QmPfCyfyLoZT398jXjJgbfdxwa3d9N91Lg4T8XdqgkSenb build/static/js/main.493cf53a.chunk.js
 466.71 KiB / 478.59 KiB   97.52%added QmYStf8YHRb5iE5nYpZ42Ei6K1zDyE2TbND1knebkGy2qk build/static/js/main.493cf53a.chunk.js.map
added QmZr8DQ21vnJSnkQhnanHM5P8fppSCt8BHAkoEqhA7S5sd build/static/js/runtime~main.d653cc00.js
 468.18 KiB / 478.59 KiB   97.82%added QmbwbsrQmRY31cCJ4dXkminAPasSe1UKSZ9mqwv2sL1u5P build/static/js/runtime~main.d653cc00.js.map
 478.59 KiB / 478.59 KiB  100.00%added QmT5m9wx8ChkC31Vnx175ShPzMpcB85AS989Hc2dPpsZsZ build/static/media/logo.5d5d9eef.svg
 478.59 KiB / 478.59 KiB  100.00%added QmNgC2EvVir5tC7jjdwHecdszpnpFPNd5M5tp42TtWYjpF build/static/css
 478.59 KiB / 478.59 KiB  100.00%added QmXqv58fNVne7xGUSECfZvw7bb6dK5v92yfyVgZQMtqjXv build/static/js
added QmUCWQYtD8otdHmda45N9svVo67nthP84heyUFydhPPzyK build/static/media
 478.59 KiB / 478.59 KiB  100.00%added QmcwHrFmu3qMK8VE3CscShr93XPxe3pcGeEpqDiCwcy5PG build/static
added QmZ5eFjjYFA7WTZhFQzko9rm9K2hUqsrB8Z81XJjtde4x3 build
```

The last line shows us the build hash and now we can access it via an endpoint, all of the following should be accessible (NOTE: A TODO is front this via a website, ideally with Cloudflare and TLS enabled: https://developers.cloudflare.com/distributed-web/ipfs-gateway/connecting-website/).

http://localhost:8080/ipfs/QmZ5eFjjYFA7WTZhFQzko9rm9K2hUqsrB8Z81XJjtde4x3
https://ipfs.io/ipfs/QmZ5eFjjYFA7WTZhFQzko9rm9K2hUqsrB8Z81XJjtde4x3/
https://cloudflare-ipfs.com/ipfs/QmZ5eFjjYFA7WTZhFQzko9rm9K2hUqsrB8Z81XJjtde4x3/

Enjoy!
