# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VS Code](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## AWS S3 Static - Bucket Policy JSON

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::<bucket-name>",
                "arn:aws:s3:::<bucket-name>/*"
            ]
        }
    ]
}
```

## AWS CodeBuild - Environment Variable

- Add environment variable: `S3_BUCKET` and set the value to your corresponding bucket name.
  - Sample:
    - Key: `S3_BUCKET`
    - Value: `s3://vue-cicd`
    - Type: `PLAINTEXT`

- Buildspec file:

```yml
version: 0.2

phases:
  install:
    commands:
      - echo "Installing dependencies..."
      - npm install

  build:
    commands:
      - echo "Building Vue app..."
      - npm run build
      
  post_build:
    commands:
      - echo "Uploading / Syncing to S3"
      - aws s3 sync ./dist $S3_BUCKET
```

## AWS CodePipeline

- Add `Build` stage
  - Set `Trigger` to `no filter`.
- Skip `Deploy` stage
- Save changes