# Git, Docker and Kubernetes Lab Exam: Questions and Answers

## 1.Create a fresh Git repository, store files locally and push to GitHub
**Question:** Explain the process of creating a fresh git repository and store files locally and in GitHub along with commands.

Git tracks changes in a local repository. GitHub hosts a remote copy. You create the local repository, commit files, create an empty repository on GitHub, link the two with a remote, and push.

1. Install Git and set your identity once.
2. Create a folder and run `git init`.
3. Add files, stage them (`git add`) and save a snapshot (`git commit`).
4. On github.com click *New repository*, name it, and leave it empty (no README).
5. Connect the remote and push.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

mkdir my-repo
cd my-repo
git init

echo "Hello Git" > readme.txt
git status
git add readme.txt
git commit -m "Initial commit"

git branch -M main
git remote add origin https://github.com/YOUR_USER/YOUR_REPO.git
git remote -v
git push -u origin main
```

- **`--global`**: Save the setting for your user account, so it applies to every repository. Without it, it applies to this repository only.
- **`git init`**: Turns the current folder into a repository by creating a hidden `.git` folder.
- **`git status`**: Shows which files are untracked, modified or staged.
- **`git add <file>`**: Moves the file into the staging area (the next commit).
- **`-m "text"`**: Commit message given inline. Without it Git opens an editor.
- **`branch -M main`**: Rename the current branch to `main`. `-M` is move/rename with force.
- **`remote add origin URL`**: Registers the GitHub URL under the nickname `origin`.
- **`remote -v`**: Verbose list of remotes with their fetch and push URLs.
- **`push -u origin main`**: Upload branch `main` to `origin`. `-u` remembers this link, so later you can type just `git push` or `git pull`.

```term
$ git init
Initialized empty Git repository in C:/Users/you/my-repo/.git/
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        readme.txt

nothing added to commit but untracked files present (use "git add" to track)
$ git add readme.txt
$ git commit -m "Initial commit"
[master (root-commit) 3f2a1b7] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 readme.txt
$ git remote -v
origin  https://github.com/YOUR_USER/YOUR_REPO.git (fetch)
origin  https://github.com/YOUR_USER/YOUR_REPO.git (push)
$ git push -u origin main
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 214 bytes | 214.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/YOUR_USER/YOUR_REPO.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

`git add`, `git branch -M` and `git remote add` print nothing when they succeed. If your Git is configured with `main` as the default, `git status` says `On branch main`.

## 2.Create a text file locally and push it to GitHub
**Question:** Create one text file in local machine and save it in local repository, push it to GitHub.

```bash
cd my-repo
echo "This file was created on my local machine" > local-file.txt

git add local-file.txt
git commit -m "Add local-file.txt"
git push origin main

git log --oneline
git status
```

- **`>`**: Shell redirect: writes the text from `echo` into the file (creates or overwrites it).
- **`push origin main`**: Send local commits on `main` to the remote `origin`.
- **`log --oneline`**: Commit history, one line per commit (short hash and message).

```term
$ git commit -m "Add local-file.txt"
[main 9c1d4e2] Add local-file.txt
 1 file changed, 1 insertion(+)
 create mode 100644 local-file.txt
$ git push origin main
Enumerating objects: 4, done.
Writing objects: 100% (3/3), 301 bytes | 301.00 KiB/s, done.
To https://github.com/YOUR_USER/YOUR_REPO.git
   3f2a1b7..9c1d4e2  main -> main
$ git log --oneline
9c1d4e2 (HEAD -> main, origin/main) Add local-file.txt
3f2a1b7 Initial commit
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Refresh the repository page on GitHub and `local-file.txt` is listed.

## 3.Create a text file in GitHub and pull it to the local repository
**Question:** Create another text file in GitHub and pull it into local repository.

1. On GitHub open the repository, click *Add file*, then *Create new file*.
2. Name it `github-file.txt`, type some text, click *Commit changes* and commit to `main`.
3. In the terminal pull the change.

```bash
cd my-repo
git pull origin main
ls
cat github-file.txt
```

- **`git pull origin main`**: Downloads new commits from `main` on `origin` (fetch) and merges them into your current branch.
- **`ls`**: Lists files. Use `dir` in Windows CMD.
- **`cat file`**: Prints the file contents.

```term
$ git pull origin main
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 987 bytes | 98.00 KiB/s, done.
From https://github.com/YOUR_USER/YOUR_REPO
 * branch            main       -> FETCH_HEAD
   9c1d4e2..b7e5a60  main       -> origin/main
Updating 9c1d4e2..b7e5a60
Fast-forward
 github-file.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 github-file.txt
$ ls
github-file.txt  local-file.txt  readme.txt
$ cat github-file.txt
Created on GitHub
```

*Fast-forward* means your branch had no new commits of its own, so Git just moved it forward to match GitHub.

## 4.Create a text file in the main branch and save it in the local repository
**Question:** Create one text file in main branch and save it in local repository.

```bash
git checkout main
git branch

echo "File created on the main branch" > main-file.txt
git add main-file.txt
git commit -m "Add main-file.txt on main"
git push origin main
```

- **`git checkout main`**: Switch to branch `main` (newer equivalent: `git switch main`).
- **`git branch`**: Lists local branches. The `*` marks the current one.

```term
$ git checkout main
Already on 'main'
Your branch is up to date with 'origin/main'.
$ git branch
* main
$ git commit -m "Add main-file.txt on main"
[main 4d8f0c3] Add main-file.txt on main
 1 file changed, 1 insertion(+)
 create mode 100644 main-file.txt
$ git push origin main
To https://github.com/YOUR_USER/YOUR_REPO.git
   b7e5a60..4d8f0c3  main -> main
```

## 5.Create a text file in "branch-1" and save it in the local repository
**Question:** Create another text file in "branch-1" branch and save it local repository.

```bash
git checkout -b branch-1

echo "File created on branch-1" > branch1-file.txt
git add branch1-file.txt
git commit -m "Add branch1-file.txt on branch-1"

git push -u origin branch-1
git branch -a
```

- **`checkout -b`**: Create a new branch and switch to it in one step (`git switch -c branch-1` does the same).
- **`push -u origin branch-1`**: Publish the new branch to GitHub and set it to track the remote branch.
- **`branch -a`**: Lists all branches, local and remote (`remotes/origin/...`).

```term
$ git checkout -b branch-1
Switched to a new branch 'branch-1'
$ git commit -m "Add branch1-file.txt on branch-1"
[branch-1 e21a9b8] Add branch1-file.txt on branch-1
 1 file changed, 1 insertion(+)
 create mode 100644 branch1-file.txt
$ git push -u origin branch-1
remote: Create a pull request for 'branch-1' on GitHub by visiting:
remote:      https://github.com/YOUR_USER/YOUR_REPO/pull/new/branch-1
To https://github.com/YOUR_USER/YOUR_REPO.git
 * [new branch]      branch-1 -> branch-1
branch 'branch-1' set up to track 'origin/branch-1'.
$ git branch -a
* branch-1
  main
  remotes/origin/branch-1
  remotes/origin/main
```

## 6.Merge both files into main, locally and in GitHub
**Question:** Merge these two files into main branch and do the same in GitHub.

### Locally

```bash
git checkout main
git pull origin main
git merge branch-1
git push origin main

ls
git log --oneline --graph --all
```

- **`git merge branch-1`**: Brings the commits of `branch-1` into the branch you are on (`main`). Add `--no-ff` to force a separate merge commit.
- **`--graph`**: Draws the branch structure as ASCII art.
- **`--all`**: Include all branches, not just the current one.

```term
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
$ git merge branch-1
Updating 4d8f0c3..e21a9b8
Fast-forward
 branch1-file.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 branch1-file.txt
$ git push origin main
To https://github.com/YOUR_USER/YOUR_REPO.git
   4d8f0c3..e21a9b8  main -> main
$ ls
branch1-file.txt  github-file.txt  local-file.txt  main-file.txt  readme.txt
$ git log --oneline --graph --all
* e21a9b8 (HEAD -> main, origin/main, origin/branch-1, branch-1) Add branch1-file.txt on branch-1
* 4d8f0c3 Add main-file.txt on main
* b7e5a60 Create github-file.txt
* 9c1d4e2 Add local-file.txt
* 3f2a1b7 Initial commit
```

It is a fast-forward because `main` had no new commits after `branch-1` was created. If both branches had changed, Git would instead print `Merge made by the 'ort' strategy.` and create a merge commit. If two branches edited the same lines you get `CONFLICT`: edit the file, delete the `<<<<<<<` / `=======` / `>>>>>>>` markers, then `git add .` and `git commit`.

### In GitHub (pull request)

1. Open the repository, click *Pull requests*, then *New pull request*.
2. Set base to `main` and compare to `branch-1`.
3. Click *Create pull request*, then *Merge pull request*, then *Confirm merge*.
4. Locally run `git checkout main` and `git pull origin main`. Expected: a `Fast-forward` or merge message listing `branch1-file.txt`.

The local merge and the GitHub merge give the same result. If you already pushed the local merge, GitHub will say there is nothing to compare. To demonstrate the GitHub merge, do it first (before the local git push origin main), then git pull.

## 7.Create a Docker container from an image
**Question:** Explain the process of creating a Docker Container using ... (the end of this question is cut off in the photo).

A Docker **image** is a read-only template. A **container** is a running instance of that image. The flow is: get the image, create and start a container, inspect it, then stop and remove it.

```bash
docker pull nginx:latest
docker run -d --name my-nginx -p 8080:80 nginx:latest
docker ps
docker logs my-nginx
curl http://localhost:8080
docker exec -it my-nginx bash
docker stop my-nginx
docker start my-nginx
docker rm -f my-nginx
docker rmi nginx:latest
```

- **`docker pull image:tag`**: Download an image from Docker Hub.
- **`-d`**: Detached: run in the background and give the terminal back.
- **`--name my-nginx`**: Give the container a name instead of a random one.
- **`-p 8080:80`**: Publish a port as `hostPort:containerPort`. Requests to your machine's 8080 go to port 80 inside the container.
- **`docker ps`**: List running containers. Add `-a` to include stopped ones.
- **`docker logs`**: Print what the container wrote to stdout/stderr. Add `-f` to follow live.
- **`exec -it`**: Run a command inside a running container. `-i` keeps input open, `-t` gives a terminal. Type `exit` to leave.
- **`stop / start`**: Stop or restart the container without deleting it.
- **`rm -f`**: Delete the container. `-f` force-removes it even if running.
- **`rmi`**: Delete an image (only possible when no container uses it).

```term
$ docker pull nginx:latest
latest: Pulling from library/nginx
a2318d6c47ec: Pull complete
Digest: sha256:9c367186df9a6b18c6735357b8eb7f407347e84aea09beb184961cb83543d...
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
$ docker run -d --name my-nginx -p 8080:80 nginx:latest
7d3c1f0a9b2e4c6d8e5f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d
$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                  NAMES
7d3c1f0a9b2e   nginx:latest   "/docker-entrypoint.…"   10 seconds ago   Up 9 seconds    0.0.0.0:8080->80/tcp   my-nginx
$ docker logs my-nginx
/docker-entrypoint.sh: Configuration complete; ready for start up
$ curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
$ docker exec -it my-nginx bash
root@7d3c1f0a9b2e:/#
$ docker stop my-nginx
my-nginx
$ docker rm -f my-nginx
my-nginx
$ docker rmi nginx:latest
Untagged: nginx:latest
Deleted: sha256:5ef79149e0ec84a7a9f9284c3f91aa3c20608f8391f5445eabe92ef07dbda03c
```

## 8.Pull the "latest" Docker image
**Question:** Write docker commands to pull "latest" docker image for ... (cut off in the photo).

Syntax: `docker pull <image>:latest`. If you leave off the tag, Docker uses `latest`. These are the images used in this sheet:

```bash
docker pull postgres:latest
docker pull traefik/whoami:latest
docker pull jkaninda/simple-api:latest
docker pull ssatyab/studentmarkservice:latest

docker images
docker image ls postgres
```

- **`postgres`**: Official image (no username prefix).
- **`traefik/whoami`**: User or organisation image: `owner/name`.
- **`:latest`**: The tag (version label). `latest` is the default tag, not always the newest release.
- **`docker images`**: Lists images stored on your machine.
- **`image ls postgres`**: Same list, filtered to the `postgres` repository.

```term
$ docker pull traefik/whoami:latest
latest: Pulling from traefik/whoami
Digest: sha256:4c3e5a1d...
Status: Downloaded newer image for traefik/whoami:latest
docker.io/traefik/whoami:latest
$ docker images
REPOSITORY                  TAG      IMAGE ID       CREATED        SIZE
postgres                    latest   a1b2c3d4e5f6   2 weeks ago    435MB
traefik/whoami              latest   b2c3d4e5f6a1   3 months ago   7.1MB
jkaninda/simple-api         latest   c3d4e5f6a1b2   2 years ago    430MB
ssatyab/studentmarkservice  latest   d4e5f6a1b2c3   1 year ago     400MB
$ docker image ls postgres
REPOSITORY   TAG      IMAGE ID       CREATED       SIZE
postgres     latest   a1b2c3d4e5f6   2 weeks ago   435MB
```

## 9.PostgreSQL container, two whoami containers, and curl
### Task 1

**Question:** Create a docker container with name "MyPostgresContainer". Set "POSTGRES_PASSWORD" environment variable to "abcd1234". Run the container on port 8844 and pull latest image of "traefik/whoami" into local machine.

```bash
docker pull traefik/whoami:latest
docker run -d --name MyPostgresContainer -e POSTGRES_PASSWORD=abcd1234 -p 8844:5432 postgres:latest
docker ps
docker logs MyPostgresContainer
docker exec -it MyPostgresContainer psql -U postgres
```

- **`-d`**: Run in the background.
- **`--name MyPostgresContainer`**: Container name (case-sensitive when you use it later).
- **`-e KEY=VALUE`**: Sets an environment variable inside the container. Postgres refuses to start without `POSTGRES_PASSWORD`. All options must come *before* the image name.
- **`-p 8844:5432`**: Host port 8844 maps to Postgres's own port 5432 inside the container.
- **`postgres:latest`**: The image. Docker pulls it automatically if it is missing.
- **`psql -U postgres`**: Open the Postgres command-line client as user `postgres`. Type `\q` to quit.

```term
$ docker run -d --name MyPostgresContainer -e POSTGRES_PASSWORD=abcd1234 -p 8844:5432 postgres:latest
Unable to find image 'postgres:latest' locally
latest: Pulling from library/postgres
Status: Downloaded newer image for postgres:latest
1f8a6b3c9d2e4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f90
$ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                    NAMES
1f8a6b3c9d2e   postgres:latest   "docker-entrypoint.s…"   8 seconds ago   Up 7 seconds   0.0.0.0:8844->5432/tcp   MyPostgresContainer
$ docker logs MyPostgresContainer
...
PostgreSQL init process complete; ready for start up.
2026-10-13 09:15:02.411 UTC [1] LOG:  database system is ready to accept connections
$ docker exec -it MyPostgresContainer psql -U postgres
psql (17.0 (Debian 17.0-1.pgdg120+1))
Type "help" for help.

postgres=#
```

### Task 2

**Question:** Run 2 containers on 2 different ports 8686 and 8787.

```bash
docker run -d --name whoami1 -p 8686:80 traefik/whoami
docker run -d --name whoami2 -p 8787:80 traefik/whoami
docker ps
```

- **`-p 8686:80`**: `traefik/whoami` listens on port 80 inside the container. Each container needs a different *host* port, but both can use 80 inside.
- **`--name`**: Two containers cannot share a name, so use whoami1 and whoami2.

```term
$ docker ps
CONTAINER ID   IMAGE            COMMAND     CREATED          STATUS          PORTS                  NAMES
8c2d1e4f5a6b   traefik/whoami   "/whoami"   5 seconds ago    Up 4 seconds    0.0.0.0:8787->80/tcp   whoami2
5f2c8a1d9e3b   traefik/whoami   "/whoami"   12 seconds ago   Up 11 seconds   0.0.0.0:8686->80/tcp   whoami1
```

### Task 3

**Question:** Check following commands and write the differences: a) curl http://localhost:8686  b) curl http://localhost:8687

```bash
curl http://localhost:8686
curl http://localhost:8687
curl http://localhost:8787
```

```term
$ curl http://localhost:8686
Hostname: 5f2c8a1d9e3b
IP: 127.0.0.1
IP: 172.17.0.3
RemoteAddr: 172.17.0.1:52814
GET / HTTP/1.1
Host: localhost:8686
User-Agent: curl/8.4.0
Accept: */*

$ curl http://localhost:8687
curl: (7) Failed to connect to localhost port 8687 after 2 ms: Could not connect to server
$ curl http://localhost:8787
Hostname: 8c2d1e4f5a6b
IP: 127.0.0.1
IP: 172.17.0.4
RemoteAddr: 172.17.0.1:52930
GET / HTTP/1.1
Host: localhost:8787
```

| Command | Result | Reason |
| --- | --- | --- |
| `curl http://localhost:8686` | Prints the whoami text: `Hostname`, container IPs, `GET / HTTP/1.1` and request headers. | Container `whoami1` publishes host port 8686, so the request reaches it. |
| `curl http://localhost:8687` | `curl: (7) Failed to connect ... Connection refused` | No container publishes 8687 (the second one uses 8787), so nothing is listening. |

`curl http://localhost:8787` works too but shows a different `Hostname` because it reaches the other container. To make (b) work: `docker run -d --name whoami3 -p 8687:80 traefik/whoami`.

#### Clean up

```bash
docker rm -f whoami1 whoami2 MyPostgresContainer
```

Output: the three names are printed back, one per line.

## 10.HTTP verbs: GET, POST, DELETE, PUT
**Question:** Explain following HTTP Verbs: GET, POST, DELETE, PUT.

| Verb | Purpose (CRUD) | Body | Safe | Idempotent | Typical success code | Example |
| --- | --- | --- | --- | --- | --- | --- |
| **GET** | Read a resource or a list (Read) | No | Yes | Yes | 200 OK | `GET /students/1` |
| **POST** | Create a new resource (Create) | Yes | No | No: repeating it creates duplicates | 201 Created | `POST /students` |
| **PUT** | Replace or update an existing resource in full (Update) | Yes | No | Yes: same request, same final state | 200 OK or 204 No Content | `PUT /students/1` |
| **DELETE** | Remove a resource (Delete) | Usually no | No | Yes: after the first call it is already gone | 200 OK or 204 No Content | `DELETE /students/1` |

*Safe* means the call does not change data on the server. *Idempotent* means calling it many times leaves the server in the same state as calling it once. You will see all four working in Q11.

## 11.Express microservice on Kubernetes, tested with Postman
**Question:** Write a Microservice to implement above operations for the following data structure Docker and orchestrate it using K8s and test Pod with Postman: { id: Integer, studentName: String, totalMarks: Integer }

A Node.js Express REST API with an in-memory list, packaged as a Docker image, run on Kubernetes by a Deployment (keeps the pod running) and a Service (stable address for it).

### Step 1: Create the project

```bash
mkdir student
cd student
npm init -y
npm install express
```

- **`npm init -y`**: Creates `package.json` with default answers (`-y` means yes to every question).
- **`npm install express`**: Downloads Express into `node_modules` and records it in `package.json`.

```term
$ npm init -y
Wrote to C:\Users\you\student\package.json:

{
  "name": "student",
  "version": "1.0.0",
  "main": "index.js",
  ...
}
$ npm install express

added 65 packages, and audited 66 packages in 4s

13 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

### Step 2: Write the service

```js
const express = require('express');
const app = express();
app.use(express.json());

let students = [];

app.get('/students', (req, res) => res.json(students));

app.get('/students/:id', (req, res) => {
  const s = students.find(x => x.id == req.params.id);
  s ? res.json(s) : res.status(404).send('Not found');
});

app.post('/students', (req, res) => {
  students.push(req.body);
  res.status(201).json(req.body);
});

app.put('/students/:id', (req, res) => {
  const i = students.findIndex(x => x.id == req.params.id);
  if (i < 0) return res.status(404).send('Not found');
  students[i] = req.body;
  res.json(students[i]);
});

app.delete('/students/:id', (req, res) => {
  students = students.filter(x => x.id != req.params.id);
  res.sendStatus(204);
});

app.listen(3000, () => console.log('Student service running on port 3000'));
```

- **`express.json()`**: Middleware that parses JSON request bodies into `req.body`. Without it POST and PUT bodies are `undefined`.
- **`students`**: The in-memory "database": an array of `{ id, studentName, totalMarks }`. It resets when the process restarts.
- **`req.params.id`**: The `:id` part of the URL, always a string, so `==` is used to compare with the numeric `id`.
- **`res.status(201)`**: Sets the HTTP status. `sendStatus(204)` sends status 204 with an empty body.
- **`app.listen(3000)`**: Start the server on port 3000.

### Step 3: Run and test locally first

```bash
node index.js
```

```bash
curl -X POST http://localhost:3000/students -H "Content-Type: application/json" -d '{"id":1,"studentName":"Asha","totalMarks":450}'
curl http://localhost:3000/students
```

- **`-X POST`**: Sets the HTTP method (GET is the default).
- **`-H "..."`**: Adds a request header. `Content-Type: application/json` tells Express the body is JSON.
- **`-d '...'`**: The request body.

```term
$ node index.js
Student service running on port 3000
$ curl -X POST http://localhost:3000/students -H "Content-Type: application/json" -d '{"id":1,"studentName":"Asha","totalMarks":450}'
{"id":1,"studentName":"Asha","totalMarks":450}
$ curl http://localhost:3000/students
[{"id":1,"studentName":"Asha","totalMarks":450}]
```

In Windows PowerShell, curl is an alias for another command. Use Git Bash, or just test with Postman. Stop the server with Ctrl+C before continuing.

### Step 4: Dockerize

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```

```text
node_modules
```

- **`FROM node:20-alpine`**: Base image: Node 20 on small Alpine Linux.
- **`WORKDIR /app`**: Working folder inside the image. Later commands run here.
- **`COPY package*.json ./`**: Copy the dependency lists first so Docker can cache the next step.
- **`RUN npm install`**: Runs at build time: installs dependencies into the image.
- **`COPY . .`**: Copy the rest of your code into `/app`.
- **`EXPOSE 3000`**: Documents the port the app uses. It does not publish it; `-p` does that.
- **`CMD [...]`**: The command run when a container starts.
- **`.dockerignore`**: Keeps your local `node_modules` out of the image (the image installs its own).

```bash
docker build -t student:1.0 .
docker run -d --name student-test -p 3000:3000 student:1.0
curl http://localhost:3000/students
docker rm -f student-test
```

- **`docker build`**: Builds an image from the Dockerfile.
- **`-t student:1.0`**: Tag (name and version) for the image. Use a version like `1.0`, not `latest`, so Kubernetes uses your local copy instead of trying to download it.
- **`.`**: Build context: the current folder, where the Dockerfile is.

```term
$ docker build -t student:1.0 .
[+] Building 14.2s (10/10) FINISHED
 => [internal] load build definition from Dockerfile
 => [1/5] FROM docker.io/library/node:20-alpine
 => [2/5] WORKDIR /app
 => [3/5] COPY package*.json ./
 => [4/5] RUN npm install
 => [5/5] COPY . .
 => exporting to image
 => => naming to docker.io/library/student:1.0
$ docker run -d --name student-test -p 3000:3000 student:1.0
3b9e5d7a1c2f4e6a8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a
$ curl http://localhost:3000/students
[]
$ docker rm -f student-test
student-test
```

### Step 5: Kubernetes files

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: student
spec:
  selector:
    matchLabels:
      app: student
  template:
    metadata:
      labels:
        app: student
    spec:
      containers:
      - name: student
        image: student:1.0
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: student
spec:
  selector:
    app: student
  ports:
  - port: 80
    targetPort: 3000
```

- **`apiVersion / kind`**: Deployment is `apps/v1`. Service (and Pod, Namespace) is `v1`.
- **`app: student`**: A label. It must match in three places: `selector.matchLabels` and `template.metadata.labels` in the Deployment, and `selector` in the Service. This is how the Service finds the pods.
- **`image`**: The image to run. Replicas default to 1 and the container port is optional, so both are left out.
- **`port: 80`**: The port the Service listens on.
- **`targetPort: 3000`**: The port the container listens on (Express uses 3000).

Forgot the YAML in the exam? Generate it: kubectl create deployment student --image=student:1.0 --dry-run=client -o yaml > deployment.yml (see Q13).

### Step 6: Deploy

**Docker Desktop Kubernetes:** it can use the image you just built, nothing else to do. **minikube:** run `minikube image load student:1.0` first. **Any other cluster:** push the image to Docker Hub (`docker tag student:1.0 YOUR_USER/student:1.0`, `docker push YOUR_USER/student:1.0`) and change the `image:` line.

```bash
kubectl apply -f deployment.yml
kubectl apply -f service.yml
kubectl get pods
kubectl get all
kubectl logs deploy/student
kubectl port-forward svc/student 8080:80
```

- **`apply -f file`**: Create or update the objects described in the file.
- **`get pods`**: Lists pods. Wait until `READY` is `1/1` and `STATUS` is `Running`.
- **`get all`**: Lists pods, services, deployments and replica sets in the namespace.
- **`logs deploy/student`**: Shows the output of a pod of that Deployment.
- **`port-forward svc/student 8080:80`**: Forward `localhost:8080` on your machine to port 80 of the Service. Keep this terminal open while using Postman.

```term
$ kubectl apply -f deployment.yml
deployment.apps/student created
$ kubectl apply -f service.yml
service/student created
$ kubectl get pods
NAME                       READY   STATUS    RESTARTS   AGE
student-6d9f7c5b8d-x2k4q   1/1     Running   0          15s
$ kubectl get all
NAME                           READY   STATUS    RESTARTS   AGE
pod/student-6d9f7c5b8d-x2k4q   1/1     Running   0          40s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP   2d
service/student      ClusterIP   10.101.23.45   <none>        80/TCP    40s

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/student   1/1     1            1           40s

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/student-6d9f7c5b8d   1         1         1       40s
$ kubectl logs deploy/student
Student service running on port 3000
$ kubectl port-forward svc/student 8080:80
Forwarding from 127.0.0.1:8080 -> 3000
Forwarding from [::1]:8080 -> 3000
```

If STATUS is ErrImagePull or ImagePullBackOff, the cluster cannot find the image. Run kubectl describe pod <pod-name> and read the Events at the bottom. Fix it with the image options at the start of this step.

### Step 7: Test in Postman

Click *New*, then *HTTP*. Choose the method, type the URL, and for POST and PUT open *Body*, select *raw*, change the type dropdown from Text to *JSON*, paste the JSON, then click *Send*. Base URL: `http://localhost:8080`.

```json
{
  "id": 1,
  "studentName": "Asha",
  "totalMarks": 450
}
```

| Request | Body | Expected status | Expected response |
| --- | --- | --- | --- |
| `POST /students` | JSON above | 201 Created | `{"id":1,"studentName":"Asha","totalMarks":450}` |
| `GET /students` | none | 200 OK | `[{"id":1,"studentName":"Asha","totalMarks":450}]` |
| `GET /students/1` | none | 200 OK | The single student object |
| `PUT /students/1` | Same JSON with `"totalMarks": 480` | 200 OK | Updated object with 480 |
| `DELETE /students/1` | none | 204 No Content | Empty body |
| `GET /students/1` (after delete) | none | 404 Not Found | `Not found` |

For PUT send the whole object, including `id`. Data lives in memory, so deleting the pod (or a restart) empties the list.

#### Clean up

```bash
kubectl delete -f deployment.yml
kubectl delete -f service.yml
```

```term
$ kubectl delete -f deployment.yml
deployment.apps "student" deleted
$ kubectl delete -f service.yml
service "student" deleted
```

## 12.Create a Docker container using a Dockerfile, then run simple-api with Compose
**Question:** Explain the process of creating a Docker Container using Dockerfile. 1. Pull latest image of "jkaninda/simple-api" 2. Download "jkaninda-simple-api-docker-compose" file 3. Run "docker compose -f jkaninda-simple-api-docker-compose up -d" 4. Test using Postman: GET "/" and GET "/books".

Is simple-api Spring Boot? Yes. Its Docker Hub description says it is a REST API with a Redis database, built with Spring Boot, for learning Docker and Kubernetes. That matters in two ways: it is a Java app listening on 8080, and the compose file you download will probably start a Redis container beside it. You do not memorise anything for steps 1 to 4. The compose file is given to you. Only if the question asks you to explain the Dockerfile process, use the 4-line Spring Boot Dockerfile below.

### Process of creating a container with a Dockerfile

1. Write a `Dockerfile` (instructions: base image, files, port, start command).
2. Build an image from it with `docker build`.
3. Start a container from that image with `docker run`.

```dockerfile
FROM eclipse-temurin:17-jre
COPY target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

```bash
mvn clean package
docker build -t simple-api-demo:1.0 .
docker run -d --name simple-api-demo -p 8080:8080 simple-api-demo:1.0
```

- **`FROM eclipse-temurin:17-jre`**: Base image with a Java 17 runtime.
- **`COPY target/*.jar app.jar`**: Copy the jar that Maven built into the image as `app.jar`.
- **`EXPOSE 8080`**: Documents Spring Boot's default port. Optional: the image runs without it.
- **`ENTRYPOINT [...]`**: The command that starts the app: `java -jar app.jar`.
- **`mvn clean package`**: Builds the project and creates the jar in `target/`.
- **`-t simple-api-demo:1.0`**: Name and tag for the image. `.` is the build context.
- **`-p 8080:8080`**: Host port : container port.

### The lab steps

#### 1. Pull the image

```bash
docker pull jkaninda/simple-api:latest
```

```term
$ docker pull jkaninda/simple-api:latest
latest: Pulling from jkaninda/simple-api
Digest: sha256:795c1337f4e0...
Status: Downloaded newer image for jkaninda/simple-api:latest
docker.io/jkaninda/simple-api:latest
```

#### 2. Download the compose file

Open the shortened link from the question in a browser and save the file in a new folder as `jkaninda-simple-api-docker-compose.yml`. Open a terminal in that folder.

#### 3. Run it

```bash
docker compose -f jkaninda-simple-api-docker-compose.yml up -d
docker compose -f jkaninda-simple-api-docker-compose.yml ps
docker compose -f jkaninda-simple-api-docker-compose.yml logs
docker compose -f jkaninda-simple-api-docker-compose.yml down
```

- **`compose`**: The Docker Compose plugin (older installs use the separate `docker-compose` command).
- **`-f file`**: Use this compose file instead of the default `docker-compose.yml`. The name must match what you saved, including `.yml`.
- **`up`**: Create the network and containers described in the file and start them.
- **`-d`**: Detached, background.
- **`ps`**: Lists the containers of this compose project. Check the `PORTS` column for the host port to use in Postman.
- **`logs`**: Shows the output of the project's containers.
- **`down`**: Stops and removes the containers and the network (run it when you are finished).

```term
$ docker compose -f jkaninda-simple-api-docker-compose.yml up -d
[+] Running 3/3
 ✔ Network simple-api_default   Created
 ✔ Container redis              Started
 ✔ Container simple-api         Started
$ docker compose -f jkaninda-simple-api-docker-compose.yml ps
NAME         IMAGE                        COMMAND                  SERVICE      STATUS         PORTS
redis        redis:latest                 "docker-entrypoint.s…"   redis        Up 5 seconds   6379/tcp
simple-api   jkaninda/simple-api:latest   "java -jar app.jar"      simple-api   Up 5 seconds   0.0.0.0:8080->8080/tcp
```

#### 4. Test with Postman

```http
GET http://localhost:8080/
GET http://localhost:8080/books
```

- `GET /`: status `200 OK` with a short JSON greeting. The project's documentation shows a message like "Hello from Simple API, everything is ok".
- `GET /books`: status `200 OK` with a JSON array of books.

If Postman says *Could not get response*, run `docker compose ... ps` to confirm the container is `Up` and the port matches.

## 13.Kubernetes commands for Deployments, Services and Pods, plus Question 9B
**Question:** Write commands for creating Deployment, Services & Pods in K8s. Question 9B: Create a k8s service pod and test using the postman: 1. Pull the image from hub.docker.com/r/ssatyab/studentmarkservice 2. Download namespace.yml, deployment.yml, service.yml.

- **Pod**: the smallest unit; one or more containers sharing network and storage.
- **Deployment**: keeps the wanted number of identical pods running and handles updates.
- **Service**: a stable address and load balancer in front of pods, selected by labels.

### Create with commands (no YAML needed)

```bash
kubectl create namespace demo-ns
kubectl run my-pod --image=nginx --port=80 -n demo-ns
kubectl create deployment my-deploy --image=nginx --replicas=3 -n demo-ns
kubectl expose deployment my-deploy --name=my-svc --port=80 --target-port=80 --type=NodePort -n demo-ns

kubectl get pods -n demo-ns
kubectl get deployments -n demo-ns
kubectl get svc -n demo-ns
```

- **`create namespace`**: A namespace is a named group of resources, like a folder.
- **`run my-pod`**: Creates a single Pod. `--image` is the container image, `--port` the container port.
- **`create deployment`**: Creates a Deployment. `--replicas=3` means keep 3 pods running.
- **`expose deployment`**: Creates a Service for it. `--port` is the Service port, `--target-port` the container port, `--name` the Service name.
- **`--type=NodePort`**: Also opens a high port (30000 to 32767) on the node. Default is `ClusterIP` (reachable only inside the cluster).
- **`-n demo-ns`**: Run the command in that namespace. Without it, the `default` namespace is used.

```term
$ kubectl create namespace demo-ns
namespace/demo-ns created
$ kubectl run my-pod --image=nginx --port=80 -n demo-ns
pod/my-pod created
$ kubectl create deployment my-deploy --image=nginx --replicas=3 -n demo-ns
deployment.apps/my-deploy created
$ kubectl expose deployment my-deploy --name=my-svc --port=80 --target-port=80 --type=NodePort -n demo-ns
service/my-svc exposed
$ kubectl get pods -n demo-ns
NAME                        READY   STATUS    RESTARTS   AGE
my-deploy-7c9d8f6b5-4hq2z   1/1     Running   0          20s
my-deploy-7c9d8f6b5-m8x7p   1/1     Running   0          20s
my-deploy-7c9d8f6b5-tz5vw   1/1     Running   0          20s
my-pod                      1/1     Running   0          45s
$ kubectl get deployments -n demo-ns
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
my-deploy   3/3     3            3           30s
$ kubectl get svc -n demo-ns
NAME     TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
my-svc   NodePort   10.98.12.34   <none>        80:31523/TCP   10s
```

### Inspect, scale, update, delete

```bash
kubectl describe pod my-pod -n demo-ns
kubectl logs my-pod -n demo-ns
kubectl scale deployment my-deploy --replicas=5 -n demo-ns
kubectl set image deployment/my-deploy nginx=nginx:1.27 -n demo-ns
kubectl rollout status deployment/my-deploy -n demo-ns
kubectl rollout undo deployment/my-deploy -n demo-ns
kubectl delete pod my-pod -n demo-ns
kubectl delete namespace demo-ns
```

- **`describe`**: Detailed info about one object. The *Events* section at the end explains most errors.
- **`logs`**: Output of the container in the pod.
- **`scale --replicas=5`**: Change the number of pods of a Deployment.
- **`set image deploy/x nginx=nginx:1.27`**: Rolling update: `container-name=new-image`. The container name is `nginx` because `create deployment` names it after the image.
- **`rollout status / undo`**: Watch a rollout, or go back to the previous version.
- **`delete namespace`**: Deletes the namespace and everything inside it.

```term
$ kubectl describe pod my-pod -n demo-ns
Name:             my-pod
Namespace:        demo-ns
Node:             docker-desktop/192.168.65.3
Status:           Running
IP:               10.1.0.15
Containers:
  my-pod:
    Image:          nginx
    Port:           80/TCP
...
Events:
  Normal  Scheduled  30s  default-scheduler  Successfully assigned demo-ns/my-pod to docker-desktop
  Normal  Pulled     29s  kubelet            Successfully pulled image "nginx"
  Normal  Started    28s  kubelet            Started container my-pod
$ kubectl logs my-pod -n demo-ns
/docker-entrypoint.sh: Configuration complete; ready for start up
$ kubectl scale deployment my-deploy --replicas=5 -n demo-ns
deployment.apps/my-deploy scaled
$ kubectl set image deployment/my-deploy nginx=nginx:1.27 -n demo-ns
deployment.apps/my-deploy image updated
$ kubectl rollout status deployment/my-deploy -n demo-ns
Waiting for deployment "my-deploy" rollout to finish: 2 out of 5 new replicas have been updated...
deployment "my-deploy" successfully rolled out
$ kubectl rollout undo deployment/my-deploy -n demo-ns
deployment.apps/my-deploy rolled back
$ kubectl delete pod my-pod -n demo-ns
pod "my-pod" deleted
$ kubectl delete namespace demo-ns
namespace "demo-ns" deleted
```

### Create with YAML (Pod, bare minimum)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: nginx
    image: nginx
```

```bash
kubectl apply -f pod.yml
kubectl get pods
kubectl delete -f pod.yml
```

```term
$ kubectl apply -f pod.yml
pod/my-pod created
$ kubectl get pods
NAME     READY   STATUS    RESTARTS   AGE
my-pod   1/1     Running   0          12s
$ kubectl delete -f pod.yml
pod "my-pod" deleted
```

No namespace is given, so this pod goes into `default`. Deployment and Service YAML are in Q11.

### Generate YAML instead of memorising it

```bash
kubectl run my-pod --image=nginx --dry-run=client -o yaml > pod.yml
kubectl create deployment my-deploy --image=nginx --dry-run=client -o yaml > deployment.yml
kubectl create service clusterip my-svc --tcp=80:80 --dry-run=client -o yaml > service.yml
```

- **`--dry-run=client`**: Build the object locally and do not create anything on the cluster.
- **`-o yaml`**: Print it as YAML.
- **`> file.yml`**: Save the printed YAML into a file. Then use `kubectl apply -f` as usual. Extra lines such as `creationTimestamp: null` and `status: {}` are harmless.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: my-deploy
  name: my-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-deploy
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: my-deploy
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```

### Question 9B: studentmarkservice, namespace, deployment and service

All three YAML files are downloaded, so nothing to memorise. The order matters: the namespace must exist before the other two files can be applied.

```bash
docker pull ssatyab/studentmarkservice:latest

kubectl apply -f namespace.yml
kubectl apply -f deployment.yml
kubectl apply -f service.yml

kubectl get ns
kubectl get pods -A
kubectl get svc -A
```

- **`docker pull`**: Downloads the image so the cluster can start it.
- **`apply -f`**: Run the three files one by one. Do not use `kubectl apply -f .`: it applies files alphabetically, so `deployment.yml` would run before `namespace.yml` and fail with "namespace not found".
- **`-A`**: All namespaces. Lets you find the new pod and service without knowing the namespace name used in the file.

```term
$ kubectl apply -f namespace.yml
namespace/<namespace-name> created
$ kubectl apply -f deployment.yml
deployment.apps/<deployment-name> created
$ kubectl apply -f service.yml
service/<service-name> created
$ kubectl get pods -A
NAMESPACE          NAME                              READY   STATUS    RESTARTS   AGE
<namespace-name>   <deployment-name>-5b7c9d8f6-abcde  1/1     Running   0          25s
kube-system        coredns-...                       1/1     Running   0          2d
```

#### Reach the service and test in Postman

Look at the `service.yml` you downloaded. If the Service type is `NodePort`, the URL is `http://localhost:<nodePort>` on Docker Desktop (for minikube run `minikube service <service-name> -n <namespace> --url`). Otherwise forward a port:

```bash
kubectl port-forward svc/<service-name> 8080:<service-port> -n <namespace-name>
```

```term
$ kubectl port-forward svc/<service-name> 8080:<service-port> -n <namespace-name>
Forwarding from 127.0.0.1:8080 -> <container-port>
Forwarding from [::1]:8080 -> <container-port>
```

Then in Postman call `http://localhost:8080/` plus the endpoints the service provides. I could not find the studentmarkservice page, so I can't tell you its exact paths: check its Docker Hub page, or the pod logs (`kubectl logs <pod-name> -n <namespace-name>`). Because the sheet describes a `{id, studentName, totalMarks}` record, the paths will be along the lines of the table in Q11. A `200 OK` JSON response means the pod and service work.

Pod stuck in ImagePullBackOff or CrashLoopBackOff? Run kubectl describe pod <pod-name> -n <namespace-name> (read Events) and kubectl logs <pod-name> -n <namespace-name>.
