# How to deploy a Node.js application in production
> Notes: This is install nodejs with sample content only. Still not yet connect the DB and Rest ful API
### Optional:
Update all app to lastest version: `sudo apt update`
# Step 1. Nodejs Install
`sudo apt install nodejs`
Version checking: `node -v`

# Step 2. npm Install

`sudo apt install npm`

--------------------
## Make a folder in Home
# Step 1. Express Install
`npm install express`
# Step 2. Create App.js file

`sudo touch app.js`

```
// Create express app
var express = require("express")
var oracledb = require("oracledb");
var app = express()
var router = express.Router();
oracledb.autoCommit = true;
var LMESConnection = {
  user: "LMES",
  password: "LMES",
  connectString: "172.30.10.49:1521/LMES",
};

// oracledb.outFormat = oracledb.OUT_FORMAT_OBJECT;
var SEPHIROTHConnection = {
  user: "SEPHIROTH",
  password: "SEPHIROTH",
  connectionString: "211.54.128.21:1521/VJEDB",
};

var HUBICConnection = {
  user: "HUBICVJ",
  password: "HUBICVJ",
  connectionString: "211.54.128.21:1521/HUBICVJ",
};

var CMMSConnection = {
  user: "CMMS",
  password: "CMMS",
  connectionString: "211.54.128.21:1521/VJCMMS",
};

// Server port
var HTTP_PORT = 3000 
// Start server
app.listen(HTTP_PORT, () => {
    console.log("Server running on port %PORT%".replace("%PORT%",HTTP_PORT))
});
// Root endpoint
app.get("/", (req, res, next) => {
    res.json({"message":"Ok"})
});

app.get("/:DB/:query/:type").post(function (request, response) {
  console.log("POST");
  var Connection = "";
  var RequestType = "";
  switch (request.url.split("/")[1]) {
    case "SEPHIROTH":
      Connection = SEPHIROTHConnection;
      break;
    case "LMES":
      Connection = LMESConnection;
      break;
    case "HUBIC":
      Connection = HUBICConnection;
      break;
    case "CMMS":
      Connection = CMMSConnection;
      break;
  }

  RequestType = request.url.split("/")[3];

  console.log("------------------------------------------------");
  oracledb.fetchAsString = [oracledb.CLOB];
  oracledb.fetchAsBuffer = [oracledb.BLOB];
  oracledb.getConnection(Connection, async function (err, connection) {
    if (err) {
      console.error(err.message);
      response.status(500).send("Error connecting to DB");
      return;
    }
    console.log("Connection Successful");

    var parameterDb = "";
    var paramValues = "";
    var paramOptions = "";
    Object.keys(request.body).forEach((key) => {
      parameterDb += `,:${key}`;
    });
    parameterDb = parameterDb.replace(/,+/, "");
    var cur = { OUT_CURSOR: { type: oracledb.CURSOR, dir: oracledb.BIND_OUT } };
    if (RequestType === "SELECT") {
      paramValues = { ...request.body, ...cur };
    } else {
      paramValues = { ...request.body };
    }
    //console.log(paramValues);
    console.log(paramValues);
    var sql = `BEGIN
                    ${request.params.query}(${parameterDb});
                  END;`;
    console.log(paramOptions);
    connection.execute(
      sql,
      paramValues,
      { outFormat: oracledb.OBJECT },
      async function (err, result) {
        if (err) {
          console.error(err.message);
          response.status(500).send(
            JSON.stringify({
              Result: "FAILED",
              RequestURL: request.url,
              Parameters: paramValues,
            })
          );
          doRelease(connection);
          return;
        }
        if (RequestType === "SELECT") {
          var resultSet = result.outBinds.OUT_CURSOR;
          const rows = await resultSet.getRows();
          await resultSet.close();
          response.send(rows);
        } else {
          response.send({
            Result: "OK",
            RequestURL: request.url,
            Parameters: paramValues,
          });
        }
        doRelease(connection);
      }
    );
  });
});

// Insert here other API endpoints

// Default response for any other request
app.use(function(req, res){
    res.status(404);
});
```



![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/API%20Folder.JPG)

# Step 3. Try To Start

`node app.js`

# Step 4. Use pm2 to managed nodejs

install pm2: `sudo npm i pm2 -g`

pm2 start: `pm2 start app.js`

pm2 save into memory: `pm2 save`

pm2 set startup: `pm2 startup ubuntu`

--------------------
## Config Nginx
# Step 1. Nginx Install
`sudo apt install nginx`
# Step 2. Create NodeApp File in path: /etc/nginx/sites-available/
`sudo touch /etc/nginx/sites-available/nodeApp`
#### Content nodeApp:
```
server {
    listen 80;
    server_name _;
    location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
            # Turn on Passenger
            #passenger_enabled on;
            # Tell Passenger that your app is a Node.js app
	    #passenger_app_type node;
	    #passenger_startup_file app.js;
        }
}
```
### Activate this configuration using the command below (In Root terminal):
`sudo ln -s /etc/nginx/sites-available/nodeApp /etc/nginx/sites-enabled`

#### Visit http://172.30.10.120/ and your application should work fine. Happy coding!
![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/Serverlive.JPG)

#### PM2 Management Server Status

![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/PM2_server_managed.JPG)

--------------------
# How to deploy a ReactJS App in production to Ubuntu Server And Deploy Build Folder.
# Step 1. Create New File Is "Your-Site-Name". For Example see image below:

![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/nginx/1.JPG)
```
server{
listen 3001;
server_name _;
   root /var/www/Your-Site-Name/html;
   index index.html index.htm;
   location / {
   #try_files $uri /index.html =404;
   }
}
```
# Step 2. Run below command for Site-Enabled:

`sudo ln -s /etc/nginx/sites-available/General-Affairs-System /etc/nginx/sites-enabled`
![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/nginx/2.JPG)

# Step 3. Check Site-Enabled folder:

![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/nginx/3.JPG)

# Step 4. Create folder with name is Your-Site-Name (GAWebsite) follow the path:

![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/nginx/4.JPG)

# Step 5. Copy all file inside BUILD folder to HTML folder: 

![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/nginx/5.JPG)

## If your operating system is window then do this: 

![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/nginx/6.JPG)

# Step 6. Restart NginX

![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/nginx/7.JPG)

# Happy Hacking!!!

![markdown](https://raw.githubusercontent.com/phuocvj/Documents/main/Images/DeployUbuntu/nginx/8.JPG)

# Login Event from UI call Login API

```
fetch(http://172.30.10.120/LMES/PKG_GA_SYSTEM_REQUEST.USER_LOGIN_SELECT, {
      method: "POST",
      mode: "cors",
      dataType: "json",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        ARG_TYPE: "Q",
        ARG_EMPID: data, //user name
        ARG_PASSWORD: base64_encode(data1), //password
        OUT_CURSOR: "",
      }),
      signal: Timeout(5).signal,
    })
      .then((response) => {
        response.json().then(async (result) => {
          if (result.length > 0) {
	   }
	})
 	});
```
