
# React + Wing Workshop

_Learn how to build a React.JS app with Wing_

This repo is a step-by-step guide for an online workshop ([eventbrite](https://www.eventbrite.com/e/winglang-react-workshop-tickets-754616256537)).

## Any issues? questions? let's talk!

Please [join Winglang's slack](https://t.winglang.io/slack), we have a dedicated channel for this workshop [#workshop-react](https://winglang.slack.com/archives/C069JFTF6AC)

## During the session, you'll learn the following

- How to develop a Wing application.
- How to build a serverless stack.
- How to set up a React website using the Wing programming model.
- How to work with DynamoDB, S3 Bucket, API Gateway, and Lambda Functions.
- How to develop and run your application locally.
- How to test your application both locally and on the cloud.
- How to produce ready-to-apply Terraform assets.
- How to create a preview environment using wing.cloud.


## The Application

A Flat File System browser and backend, the file system allows only one hirarchy of folders, each folder can have multiple files. 

See [Demo hosted on AWS](https://d9lecxfcp4503.cloudfront.net/)

![winglang file system drawio (1)](https://github.com/ekeren/react-wing-workshop/assets/1727147/455df542-ec23-4f4a-ada8-ef941de7b53b)


### Workshop Sessions

1. [Why Even Fly? I'm Comfortable Where I Am](https://raw.githubusercontent.com/ekeren/react-wing-workshop/main/assets/why.pdf) - Probelms with Serverless, and the Wing approach.
2. [It’s Nearly Time to Fly](./01-setup.md) - Install and verify the Winglang toolchain is working on our computer.
3. [Prepare for Liftoff](./02-api.md) - Your second Wing program, getting to know cloud.Api.
4. [Please Fasten Your Seat Belt](./03-react.md) - Blazing fast iteration cycles with React & Winglang.
5. [We Are Now Cruising](./04-preview.md) - Setting up preview environments on PR.
6. [Entering an Area of Turbulence](./05-db.md) - Modeling a FlatFileSystem using DynamoDB & cloud.Bucket.
7. [Thank You for Flying with Us](./06-wrap.md) - Wrapping it all together.

 
### Some General Resources:

- [Learn about Inflights](https://www.winglang.io/docs/concepts/inflights) - Preflight and Inflight are Winglang's most important core concepts.
- [Cheat Sheet](./cheatsheet.md) - Let's learn Wing in 5 minutes, shall we?
- [Language Reference](https://www.winglang.io/docs/language-reference) - A complete language reference for Winglang.
- [Playground](https://www.winglang.io/play/) - Write and test Wing code online.
