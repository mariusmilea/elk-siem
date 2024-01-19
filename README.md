# elk-siem
SIEM(Security information and event management) tool implemented using the ELK stack.

To start the stack type:
```bash
docker-compose up
```

To stop the stack and remove the volumes:
```bash
docker-compose down -v
```

# 3
A screenshot with the SIEM solution and the ingrested access logs can be seen below.
![screenshot](/elk-siem-screenshot.png?raw=true "SIEM screenshot")

The nodejs app needs the following code to enable access log writing:
```js
var morgan = require('morgan')
var path = require('path')
const json = require('morgan-json');

// create a write stream (in append mode)
var accessLogStream = fs.createWriteStream(path.join(__dirname, 'access.log'), { flags: 'a' })

// add headers to logger
morgan.token('req-headers', function(req,res){
 return JSON.stringify(req.headers)
})

morgan.token('res-headers', function(req,res){
 return JSON.stringify(res.headers)
})

// setup the logger
const format = json(':method :url :status :req-headers :res-headers');

app.use(morgan(format, { stream: accessLogStream }))
```

# 4
The origin field had to be added into the access logs.

A KQL query that could show us all unauthorised origins would look like this:
```
NOT origin: https\://www.nc.com\:3000
```
 
# 5
Elastalert is a very nice tool that could be used to create alerts based on elasticsearch logs.

This time, however, I'll use the Alerts functionality of Kibana. A screenshot can be seen below.
![screenshot](/cors-alert.png?raw=true "CORS alert screenshot")
