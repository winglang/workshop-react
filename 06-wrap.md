
In this session, we will use the FlatFileSystem module in our API and add the relevant code on the client to interact with the REST API provided by `cloud.API`.

## Instructions 

1. Download and Unzip 
   [assets/client.zip](https://raw.githubusercontent.com/ekeren/react-wing-workshop/main/assets/client.zip)
   and overwrite the current `client` directory. The code includes interaction with the REST API, React routes, some CSS design, etc.

2. Install new client dependencies:
   ```
   cd client
   npm install
   ```

3. The new React app expects more routes. Paste the following code (that uses `FlatFileSystem`) into `backend/main.w`:
   ```ts
   bring ex;
   bring cloud;
   bring "./flat-file-system.w" as f;

   let fileStorage = new f.FlatFileSystem();

   let website = new ex.ReactApp(
     projectPath: "./../client",
     localPort: 4002
   );
   
   let api = new cloud.Api(
     cors: true
   );

   website.addEnvironment("apiUrl", api.url);

   api.get("/api/folders", inflight (req) => {
     return {
       status: 200,
       body: Json.stringify(fileStorage.listFolders())
     };
   });

   api.post("/api/folders", inflight (req) => {
     if let body = req.body {
       let folder = Json.parse(body).get("folder").asStr();
       fileStorage.createFolder(folder);
       return {
         status: 200,
         body: str.fromJson(folder)
       };
     }
     return {
         status: 500,
         body: "missing body"
       };
   });

   api.get("/api/folders/:folder", inflight (req) => {
     let folder = req.vars.get("folder");
     return {
       status: 200,
       body: Json.stringify(fileStorage.listFiles(folder))
     };
   });

   api.get("/api/folders/:folder/:file", inflight (req) => {
     let folder = req.vars.get("folder");
     let file = req.vars.get("file");
     return {
       status: 200,
       body: fileStorage.getFile(folder, file)
     };
   });

   api.put("/api/folders/:folder/:file", inflight (req) => {
     let folder = req.vars.get("folder");
     let file = req.vars.get("file");
     let content = req.body ?? "";
     fileStorage.createFile(folder, file, content); 
     return {
       status: 200,
       body: Json.stringify({filename: file, content: content})
     };
   });
   ```

4. Start the service using:
   ```
   BROWSER=none wing run backend/main.w
   ```

5. Compile it to Terraform for AWS and apply it (requires Terraform CLI with configured AWS credentials):
   ```
   wing compile -t tf-aws backend/main.w
   cd backend/target/main.tfaws
   terraform init
   terraform apply
   ```

🚀 Congratulations, you now have a running React + Wing website on AWS! 🚀
