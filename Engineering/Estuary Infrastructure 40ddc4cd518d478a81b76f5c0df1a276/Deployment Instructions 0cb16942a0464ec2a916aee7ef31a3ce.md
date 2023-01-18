# Deployment Instructions

# Estuary:

NB: Deploying code changes will cause some service downtime. The estuary process is managed by tmux

## Communication

**Pre-release:** Communicate the release to #ecosystem-dev chat.

**Post-release:** Create a github discussion and post release info on #ecosystem-dev chat.

## Tag Build

- Pick a version. e.i vX.Y.Z.
- Create a tag of the master branch to create a cut-through of the features that’ll be included on the next release.
    - Run these locally
        - `git tag vX.Y.Z`
        - `git push origin vX.Y.Z`
    - Go to the github UI and create a release
        - Click “Generate release notes” to automatically populate release notes.
        - Check the “create a discussion for this release box”
        
        ![Screen Shot 2022-07-18 at 3.59.48 PM.png](Deployment%20Instructions%200cb16942a0464ec2a916aee7ef31a3ce/Screen_Shot_2022-07-18_at_3.59.48_PM.png)
        

### Estuary Main API Node Deployment:

1. Login to `api.estuary.tech` using `root` for now
`ssh root@api.estuary.tech` or `ssh -i <path to id_rsa file> root@api.estuary.tech`
2. Create a `tmux` session if there is none.
    1. Basic `tmux commands` are
        1. ctrl+b and c = new tab
        2. ctrl+b and d = exit
        3. ctrl+b and <tab number> to go the tab.
    2. cd into the `/mnt/build/estuary` directory and pull the latest code
        
        `git fetch --tags`
        
        `git checkout vX.X.X`
        
        `git log` - verify the last commit is consistent with the tag.
        
    3. build the binaries 
        
        `make build` or `make estuary` 
        
    4. Stop the API node `systemctl stop estuary.service`
    5. Copy the `estuary` binary to `/mnt/bin`
    6. Start the API node `systemctl start estuary.service`

### **Running SQL Queries:**

1. Prepare the SQL script
2. open postgres on another tmux tab
3. Access `psql` and login as postgres user and switch to estuary
    1. `sudo -u postgres psql`
    2. `\c estuary`
    3. `\i <path to sql file>` 

### Stop/Start/Restarting Estuary:

1. Login to `api.estuary.tech` using `root` for now
`ssh root@api.estuary.tech` or `ssh -i <path to id_rsa file> root@api.estuary.tech`
2. Create a `tmux` session if there is none.
    1. Basic `tmux commands` are
        1. ctrl+b and c = new tab
        2. ctrl+b and d = exit
        3. ctrl+b and <tab number> to go the tab.
    2. Stop the API node `systemctl stop estuary.service`
    3. Start the API node `systemctl start estuary.service`
    4. Restart the API node `systemctl restart estuary.service`

### Changing/Updating Estuary API config:

1. Login to `api.estuary.tech` using `root` for now
`ssh root@api.estuary.tech` or `ssh -i <path to id_rsa file> root@api.estuary.tech`
2. Create a `tmux` session if there is none.
    1. Basic `tmux commands` are
        1. ctrl+b and c = new tab
        2. ctrl+b and d = exit
        3. ctrl+b and <tab number> to go the tab.
    2. Go the `/mnt/run`
    3. Edit the `/mnt/run/run.sh` and change the parameters
    4. Restart the API node `systemctl restart estuary.service`

### Restart Nginx:

1. Login to `api.estuary.tech` using `root` for now
`ssh root@api.estuary.tech` or `ssh -i <path to id_rsa file> root@api.estuary.tech`
2. Run `systemctl restart nginx.service`

# Shuttle:

### Shuttle-1,2,4,5

### Deploying code changes to Shuttle:

1. login to shuttle at `shuttle-<number>.estuary.tech` using `ubuntu` (for shuttle 4 and 5), `root` (for shuttle 1), and `estuary` (for shuttle 2)
`ssh <user>@shuttle-<number>.estuary.tech` or `ssh -i <path to id_rsa file> <user>@shuttle-<number>.estuary.tech`
2. cd into the `estuary` directory and pull the latest code
    
    `cd estuary`
    
    `git pull`
    
    On shuttle-1, estuary is in `/home/why/code/estuary`
    
3. build the binaries 
    
    `make build` or `make shuttle` only? I am not sure if only `make shuttle` is enough
    
4. copy shuttle binary into `usr/local/bin`
5. For shuttle-4 and shuttle-5 using systemd: Stop the shuttle service via systemctl
`sudo systemctl stop estuary-shuttle.service`
For shuttle-1 and shuttle-2, use tmux the same way as the main node.
6. Copy the new binary 
`cp estuary-shuttle /usr/local/bin`
7. Restart the shuttle via systemctl
`sudo systemctl restart estuary-shuttle.service`
8. use `htop` to check that the shuttle service is running fine
9. use `logout` to exit the ssh session

### Restarting/kicking Shuttle:

1. login to shuttle at `shuttle-<number>.estuary.tech` using `ubuntu`
`ssh ubuntu@shuttle-<number>.estuary.tech` or `ssh -i <path to id_rsa file> ubuntu@shuttle-4.estuary.tech`
2. Restart the shuttle via systemctl
`sudo systemctl restart estuary-shuttle.service`
3. use `htop` to check that the shuttle service is running fine
4. use `logout` to exit the ssh session

### Deploying Filecoin FFI changes to Shuttle:

1. login to shuttle at `shuttle-4.estuary.tech` using `ubuntu`
`ssh ubuntu@shuttle-4.estuary.tech` or `ssh -i <path to id_rsa file> ubuntu@shuttle-4.estuary.tech`
2. todo

### Accessing Shuttle Database:

1. login to shuttle at `shuttle-<number>.estuary.tech` using `ubuntu` (for shuttle 4 and 5), `root` (for shuttle 1), and `estuary` (for shuttle 2)
`ssh <user>@shuttle-<number>.estuary.tech` or `ssh -i <path to id_rsa file> <user>@shuttle-<number>.estuary.tech`
2. `sudo su - estuary`
3. `psql`

### Changing/Updating Shuttle config:

NB: The `config.env` and `log.env` files are located in the `scripts/est-shuttle-service` directory of the repo, but are located in `/etc/estuary-shuttle/` directory in the shuttle server. 

1. login to shuttle at `shuttle-4.estuary.tech` using `ubuntu`
`ssh ubuntu@shuttle-4.estuary.tech` or `ssh -i <path to id_rsa file> ubuntu@shuttle-4.estuary.tech`
2. use `sudo nvim /etc/estuary-shuttle/config.env` to edit the ENV variables as needed
3. use `sudo nvim /etc/systemd/system/estuary-shuttle.service` to edit the cli flags to match ENV variables where needed. You can also add, removed, or change flags that do not require ENV values 
4. Restart the shuttle service via systemctl
`sudo systemctl restart estuary-shuttle.service`
5. use `systemctl status estuary-shuttle.service` to check that the shuttle service is running fine
6. use `logout` to exit the ssh session

### Shuttle-6

### Shuttle-7

### Shuttle-8

## Upload Proxy

## Backup Server

## Prometheus

## Shuttle-6

```markdown
Just a quick deployment script for shuttle-6.

## Step 0: logon to server as root.
```
ssh i- ~/.ssh/<your key> root@shuttle-6.estuary.tech
```

## Step 1: Build and Load
```
tmux a
```
ctrl + b + c for new session

Go to `$HOME/deploy/`

run `./build-cp.sh -t <tag> // ./build-cp.sh -t v0.1.8`

This runs the build, creates the binary and copy it over to the proper bin folder.

## Step 2: Run run.sh
Either leave tmux session or create a nohup session to run it on the background.
```
./run.sh
```
OR
```
nohup ./run.sh &
tail -f nohup.out
```

## Step 3: Test
curl or wget
- https://shuttle-6.estuary.tech/net/addrs
- https://shuttle-6.estuary.tech/health
```

# Shuttle-Proxy:

### Deploying code changes to Shuttle-Proxy:

todo

### Restarting/kicking Shuttle-Proxy:

todo

# Estuary Staging (testing) box

Changes on the `dev` estuary branch on github are automatically deployed upon push. To make sure a staging box correctly deploys the code you have to do the following:

1. Clone application-research/estuary-docker on the `/root` folder of the staging box
2. Change these on `/root/estuary-docker/docker-compose.yml`:
    - `ESTUARY_API_LISTEN=0.0.0.0:80` (put estuary-www on port 80)
    - `ESTUARY_HOSTNAME=http://staging.estuary.tech` (everywhere)
    - `ESTUARY_API=http://estuary-main:3004` to`http://staging.estuary.tech:3004`
    - comment out the estuary-shuttle part

**TODO:** use postgres instead of estuary.db (see [Add example of postgres configuration on docker-compose.yml estuary-docker#4](https://github.com/application-research/estuary-docker/issues/4))

### Manual push and loging to instance

For estuary staging instance, login is

```
Host estuary-staging
    Hostname staging.estuary.tech
    User root
		IdentityFile ~/.ssh/id_rsa
```

Or you could also use the CLI

```jsx
ssh root@staging.estuary.tech -i ~/.ssh/id_rsa
```

Your ssh key needs to be added (ping @Gabriel de Melo Cruz or anyone from the team). If you’re on the [a-team](https://github.com/orgs/application-research/teams/a-team/) of the [application-research org on github](https://github.com/orgs/application-research/) you might’ve already been added by [this amazing script](https://gist.github.com/gmelodie/7ca4f25c1fb2b2c7c9e0e9e36338b4d1)

After that you can use `tmux attach` to attach to the tmux session running the instance, and use the following to (re)start the instance:

```jsx

./estuary --database="postgres=host=127.0.0.1 user=postgres password=postgres dbname=estuary" --datadir=/data/estuary --apilisten=0.0.0.0:3004 --logging=false --fail-deals-on-transfer-failure --disable-deal-making=false --disable-content-adding=false --disable-local-content-adding=false
```

# Autoretrieve

For main autoretrieve instance, login is

```
Host autoretrieve
    Hostname 145.40.90.135
    User estuary
		IdentityFile ~/.ssh/id_rsa
```

Or you could also use the CLI

```jsx
ssh estuary@145.40.90.135 -i ~/.ssh/id_rsa
```

Your ssh key needs to be added (ping @Elijah Arita or anyone from the team). If you’re on the [a-team](https://github.com/orgs/application-research/teams/a-team/) of the [application-research org on github](https://github.com/orgs/application-research/) you might’ve already been added by [this amazing script](https://gist.github.com/gmelodie/7ca4f25c1fb2b2c7c9e0e9e36338b4d1)

0. If tmux is not already running (check with `tmux list-sessions`), then create a new session with `tmux` 
1. Attach to the running tmux session (if you just created one you’re already attached) with `tmux attach`

1. `cd /home/estuary/autoretrieve` and make sure it’s the latest version by checking out and pulling master: `git checkout master && git pull`. Now build autoretrieve with `make clean && make`
2.  Configure autoretrieve (if you haven’t completely wiped the configs, you probably want to skip this section and use what’s already configured, but keep reading if you’re starting a new box)
    1. `./autoretrieve gen-config`
    2. Register autoretrieve to an estuary node (this example registers it to the main estuary node at `[api.estuary.tech](http://api.estuary.tech)`: `./autoretrieve register-estuary [https://](http://localhost:3004/)api.estuary.tech YOUR-ESTUARY-ADMIN-API-KEY`
3. Finally, run autoretrieve: `./autoretrieve` and detach from the tmux session

 
Obs: data dir is at `/mnt/data` and config is `config.yaml` inside that dir. the first two config keys have the estuary/indexer config stuff

# Monitoring with Grafana and Pprof

## Monitoring performance metrics with Pprof

### Check the Heap Profile of Estuary:

1. login to `api.estuary.tech` remotely (`ssh api.estuary.tech`) or just run step 2 locally on your machine(preferred approach)
2. using the Golang the profiling `tool`, point to the public estuary pprof endpoint
`go tool pprof <https://api.estuary.tech/debug/pprof/heap`>
3. run profiling commands to determine memory usage, etc (article on pprof usage: [https://jvns.ca/blog/2017/09/24/profiling-go-with-pprof/](https://jvns.ca/blog/2017/09/24/profiling-go-with-pprof/))
    
    ```
    (pprof) top20
    (pprof) list pinContentOnShuttle
    
    ```
    

### Check all Goroutines in Estuary:

This is the general path to take when something strange is happening. One can pull the go routines and bring up the heat dump and investigate from there.
This is a rich tool that provides potential performance incites as well as runtime bugs.zf

1. `curl <https://api.estaury.tech/debug/pprof/goroutine\\?debug=2> > goros`
2. Download and build `stackparse` (helps to parse and display the stack in a nice manner)
`git clone https://github.com/whyrusleeping/stackparse`
`cd stackparse && go build`
3. Parse the goroutines file with `stackparse`:
`cat goros | ./stackparse --summarycat goros | ./stackparse --fm=pinContentOnShuttle`

## Monitoring logs with Grafana

Grafana contains a lot of information about the shuttles and primary estuary service: disk usage, memory usage, etc.

Dashboards: [https://protocollabs.grafana.net/d/9AtDvgG7k/estuary?orgId=1](https://protocollabs.grafana.net/d/9AtDvgG7k/estuary?orgId=1)
Logs (text-based): [https://protocollabs.grafana.net/explore?orgId=1&left={"datasource":"grafanacloud-protocollabs-logs","queries":[{"refId":"A","expr":"{job%3D\"estuary\"}","queryType":"range"}],"range":{"from":"now-1h","to":"now"}}](https://protocollabs.grafana.net/explore?orgId=1&left=%7B%22datasource%22:%22grafanacloud-protocollabs-logs%22,%22queries%22:%5B%7B%22refId%22:%22A%22,%22expr%22:%22%7Bjob%3D%5C%22estuary%5C%22%7D%22,%22queryType%22:%22range%22%7D%5D,%22range%22:%7B%22from%22:%22now-1h%22,%22to%22:%22now%22%7D%7D)

Understanding the log search thingy: On Grafana, go to Explore, then on “labels” choose “job” and make it equal “estuary”.