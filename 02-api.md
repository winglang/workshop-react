
In this section, we will create a simple API Gateway that responds to `GET /hello`.

## Instructions

1. Replace `backend/main.w` with the following content:
   ```ts
   bring cloud;
   bring http;
   bring expect;

   let api = new cloud.Api();

   test "GET hello" {
     let url = api.url;
     let res = http.get("{url}/hello");
     expect.equal(res.status, 200);
     expect.equal(res.body, "React Wing Workshop");
   }
   ```
2. Run it locally:
   ```sh
   wing run backend/main.w
   ```
3. Run the test from the Wing Console (on the left-hand side) and examine the test result in the logs area (at the bottom).

   ![image](https://github.com/ekeren/react-wing-workshop/assets/1727147/0d1a05a0-1f81-4514-a30a-9967cda0b200)

4. Can you fix the code so the test passes? (Hint: See [cloud.Api docs](https://www.winglang.io/docs/standard-library/cloud/api))

   <details>
     <summary>Solution</summary>
     
     
     api.get("/hello", inflight () => {
       return {
         status: 200,
         body: "React Wing Workshop"
       };
     });
     
     
   </details>

You can now use different options to verify that your API gateway is working:
- When `wing run backend/main.w` is running, you can:
  - Run tests from the Wing Console.
  - Interactively test the `/hello` endpoint from within the Wing Console (using the right-hand side widget).
  - Interactively test the `/hello` endpoint using your browser (by copying the URL from the right-hand side widget).
- Run tests using the CLI `wing test backend/main.w`.

🚀 Great, you just finished creating a simple "Hello World" web service. 🚀 
