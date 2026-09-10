# aaaaaaaaa
frustration relief, for users. frustration causing, for me

## what it does

hold or tap to let out a scream. watch it escalate through 10 levels — text grows, colors shift, optional sound cranks up. when you hit max, add your scream to the shared wall under a randomly generated wailing username ("Howling Teacup", "Caterwauling Kazoo", etc.). view the wall to see everyone else's screams.

### the app

just open `aaaaaaaaa-app.html` and scream. sound is off by default (toggle with the button).

### the backend

the wall needs a Node.js server to persist entries. follow the steps below.

## deploy the backend

1. **install dependencies:**
   
cd server
npm install

2. **start the server:**

node server.js

   it listens on port 3001 by default. set `PORT=xxxx` to change it.

3. **keep it running** (use pm2 for persistence):

npm install -g pm2
pm2 start server.js --name aaaaaaaaa-wall
pm2 save
pm2 startup


4. **point nginx at it.** add this to your nginx config:
```nginx
   location /api/ {
       proxy_pass http://127.0.0.1:3001;
       proxy_http_version 1.1;
       proxy_set_header Host $host;
       proxy_set_header X-Real-IP $remote_addr;
   }
```
   then:

sudo nginx -t
sudo systemctl reload nginx


## tech stack

**app:**
- html
- vanilla javascript
- Web Audio API (for optional sound)

**backend:**
- Node.js
- Express.js
- JSON file storage (no database)

## api

- `GET /api/wall` → returns `{ entries: [{ name, level, ts }, ...] }` (newest first, capped at 50)
- `POST /api/wall` with `{ level: 0-10 }` → generates random name server-side, stores entry, returns it

## links

- [live site](https://pointlesslittleapps.com/apps/aaaaaaaaa.html)
- [all apps](https://github.com/pointlesslittleapps)
