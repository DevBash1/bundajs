# [BundaJS](https://app.privjs.com/my-package?pkg=bunda)

[BundaJS](https://app.privjs.com/my-package?pkg=bunda) is a utility library that simplifies the process of sending HTTP requests through Axios from Socket.io to an Express.js server. It facilitates seamless communication between your Socket.io-powered application and Express.js backend.

## Features

-   Send HTTP requests from Socket.io to Express.js using Axios.
-   Simplifies handling communication between frontend (Socket.io) and backend (Express.js).

## Installation

```bash
npm login --registry https://r.privjs.com
npm i -S bunda --registry https://r.privjs.com
```

[https://app.privjs.com/my-package?pkg=bunda](https://app.privjs.com/my-package?pkg=bunda)

## Usage

### On the Server (Express.js)

```javascript
const http = require("http");
const express = require("express");
const bunda = require("bunda");

const app = express();
const server = http.createServer(app);

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

bunda(server, app, (io) => {
    io.on("connection", (socket) => {
        console.log(socket.id);
    });
});

app.post("/", (req, res) => {
    const { name } = req.body;
    res.status(200).send({ message: `Hello ${name}, from BundaJS App` });
});

server.listen(3000, () => {
    console.log("Bunda App is running...");
});
```

### On the Client (React)

```javascript
import { useEffect, useState } from "react";
import axios from "axios";
import Bunda from "bunda";

function App() {
    const [message, setMessage] = useState("Click to send request");

    useEffect(() => {
        const bunda = new Bunda("localhost:3000"); // Create BundaJS instance
        bunda.axios(axios);
    }, []);

    const testBunda = async () => {
        try {
            const response = axios.post("/", {
                name: "John Doe",
            });

            setMessage(response.data.message);
        } catch (err) {
            throw err;
        }
    };

    return (
        <>
            <button onClick={testBunda}>{message}</button>
        </>
    );
}

export default App;
```

## API Reference (Frontend)

### `(new Bunda(url[, config]).axios(axiosInstance)`

Configure Axios to send requests through BundaJS.

-   `url`: The socket endpoint to route request through.
-   `config` (optional): Socket.io configuration.
-   `axiosInstance`: Axios Instance.

## API Reference (Backend)

### `Bunda(httpServer[, ExpressApp, Socket.IoCallBack]);`

Configure BundaJS to use ExpressJS.

-   `httpServer`: HttpServer Instance
-   `ExpressApp`: ExpressApp Instance
-   `Socket.IoCallBack` (optional): Socket.io callback function.

## Contributing

Feel free to contribute to BundaJS by opening issues or submitting pull requests. Contributions are always welcome!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
