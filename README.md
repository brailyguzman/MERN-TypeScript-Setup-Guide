# MERN TypeScript Project Setup Guide
![image](https://github.com/brailyguzman/MERN-TypeScript-Setup-Guide/assets/94770717/d4c2e830-4030-4664-b5f4-e4f629cd4456)


Hello, I'm Braily Guzman. Welcome to my guide on setting up a MERN stack project using TypeScript. This guide is designed to help both beginners and experienced developers to set up a fully functional development environment for MERN stack development.

If you find this guide helpful, please consider giving it a star on [GitHub](https://github.com/brailyguzman/MERN-TypeScript-Setup). Your support helps me to continue creating more guides like this. If you have any suggestions or improvements, feel free to contribute or open an issue. Let's learn and grow together in our coding journey!
Now, let's get started with the setup.

## Requirements

-   [Node.js](https://nodejs.org/en/download/) (version 14 or above recommended)
-   [MongoDB database or MongoDB Community Server](https://www.mongodb.com/try/download/community)
-   Install TypeScript using the following command:
    ```bash
    npm install -g typescript
    ```

## Root

1. Run the following command to automatically create the `package.json` file:

    Explanation: This command initializes a new Node.js project and creates a package.json file with default values.

    ```bash
    npm init -y
    ```

## Client

For the client, you can use either Vite or Create React App. Here are the instructions for both:

### Using Vite

1. On your **root folder**, type the following command:

    Explanation: This command creates a new Vite application in a directory named `client`.

    ```bash
    npx create-vite@latest client
    ```

2. You will see different options, **select React**.
3. Choose the option: **TypeScript + SWC.**
4. Once it finishes, use the following command to navigate to the **client directory**.

    ```bash
    cd client
    ```
5. Once in the client directory, run the following command to **install all dependencies**.

    Explanation: This command installs the dependencies listed in the package.json file.

    ```bash
    npm install
    ```

### Using Create React App

1. On your **root folder**, type the following command:

    Explanation: This command creates a new Create React App application in a directory named `client`.

    ```bash
    npx create-react-app client --template typescript
    ```

## Server

1. On your **root directory**, let’s create the `server` directory.

    Explanation: This command creates a new directory named server.

    ```bash
    mkdir server
    ```

2. Navigate to the `server` directory using the following command:

    ```bash
    cd server
    ```

### The following commands need to be run inside the server directory we’ve created.

3. Run this command to automatically create a `package.json` file.

    ```bash
    npm init -y
    ```

4. Now, **run the following commands** to install our dependencies

    Explanation: These commands install the necessary dependencies for our server. cors is used for enabling CORS, dotenv for loading environment variables, express for building the server, and mongoose for connecting to MongoDB. The development dependencies include TypeScript and the type definitions for our packages, as well as nodemon and ts-node for running our server during development.

    ```bash
    npm install cors dotenv express mongoose
    npm install -D typescript @types/express @types/cors @types/node @types/mongoose nodemon ts-node
    ```

5. Create a `.gitignore` file and add the following lines to it:

    Explanation: The .gitignore file specifies intentionally untracked files that Git should ignore.

    ```bash
    node_modules
    .env
    dist/
    ```

6. Now, let’s setup **TypeScript**, create a file named `tsconfig.json`

    Explanation: The `tsconfig.json` file is a configuration file for TypeScript. It specifies the root files and the compiler options required to compile the project.

7. **Copy and paste** the following configurations:

    ```json
    {
        "compilerOptions": {
            "target": "es2016",
            "jsx": "preserve",
            "module": "commonjs",
            "allowJs": true,
            "outDir": "./dist",
            "esModuleInterop": true,
            "forceConsistentCasingInFileNames": true,
            "strict": true,
            "skipLibCheck": true
        },
        "exclude": ["node_modules", "dist", "client"]
    }
    ```

8. Create a directory named `src` inside your server directory using the following command:

    ```bash
    mkdir src
    ```

### The following commands need to be run inside the `src` directory we’ve created.

9. Create a file called `server.ts`

    Explanation: This `server.ts` file sets up an Express server that connects to a MongoDB database and starts listening on a specified port.

10. Here’s a basic implementation of what the `server.ts` file should look like:

    ```tsx
    import express, { Express, Request, Response } from 'express';
    import cors from 'cors';
    import mongoose from 'mongoose';
    import dotenv from 'dotenv';

    dotenv.config();

    const app: Express = express();

    app.use(cors());
    app.use(express.json());
    app.use(express.urlencoded({ extended: true }));

    const uri: string =
        process.env.MONGODB_URI || 'mongodb://localhost:27017/your-app';

    (async () => {
        try {
            await mongoose.connect(uri);
            console.log('Connected to the database');
        } catch {
            console.log('Error connecting to the database');
        }
    })();

    app.get('/health', (_req: Request, res: Response) => {
        res.status(200).send('Server is running');
    });

    const PORT: string | number = process.env.PORT || 3000;

    app.listen(PORT, () => {
        console.log(`Server is running on PORT: ${PORT}`);
    });
    ```

11. Let’s head back to the server directory using the following command:

    ```bash
    cd ..
    ```

    Explanation: This command changes the current directory to the parent directory.

### The following content is for the server directory.

12. Now, let’s head over to our `package.json` file in the **server directory**.
13. Let’s add a command so that we can run **nodemon with ts-node** for our development.
14. **Copy and paste** this line on the script part of your `package.json`:

    ```tsx
    "scripts": {
        "start": "ts-node src/server.ts",
        "build": "tsc",
        "dev": "nodemon src/server.ts"
    },
    ```

15. Once this is done, let’s head over to the root directory with the following command:

    ```bash
    cd ..
    ```

## Running our project

### The following content is for the project’s root directory.

1. Run the following command to install the development dependency `concurrently`

    Explanation: `concurrently` is a package that allows you to run multiple npm scripts concurrently (at the same time).

    ```bash
    npm install --save-dev concurrently
    ```

2. Navigate to the package.json file in your project's root directory. This file contains metadata about your project and its dependencies.

3. Depending on the tool you used to create your client (either Vite or Create React App), copy the corresponding code block and paste it into the scripts section of your package.json file.

-   #### Vite
    ```json
    "scripts": {
        "client": "cd client && npm run dev",
        "server": "cd server && npm run dev",
        "dev": "concurrently \"npm run server\" \"npm run client\""
    },
    ```
-   #### Create React App
    ```json
    "scripts": {
        "client": "cd client && npm run start",
        "server": "cd server && npm run dev",
        "dev": "concurrently \"npm run server\" \"npm run client\""
    },
    ```

4. Now, let’s run our **project** using the following command:

    ```bash
    npm run dev
    ```

5. Now, our `client` and `server` should be running **concurrently**. You should see the following output in your terminal:

    ```bash
    [0] [nodemon] starting `ts-node src/server.ts`
    [1]
    [1]   VITE v5.1.3  ready in 248 ms
    [1]
    [1]   ➜  Local:   http://localhost:5173/
    [1]   ➜  Network: use --host to expose
    [0] Server is running on PORT: 3000
    [0] Connected to the database
    ```
