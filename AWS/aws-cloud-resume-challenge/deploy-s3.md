# Deploying S3 bucket for the website.

![image](https://github.com/hhphu/Cloud/assets/45286750/bd45bfe6-fc74-4b70-a70a-d1382b813d59)

# Introduction
In this post, I will go over 3 different ways to deploy S3 buckets: Using the AWS console, using AWS SAM and Terraform. 

# Deploying S3 bucket using AWS Console
From AWS consle, go to S3 and click "Create bucket" button
  
  ![image](https://github.com/hhphu/Cloud/assets/45286750/1ab4a824-929c-481d-a7a1-1f4875512b05)
  
In the **Block all public access** section, the box is checked. This ensures that the bucket is not exposed to the public. 
Once the bucket is created, select the bucket and go to its **Properties** tab. Scroll down to the **Static website hosting** at the bottom and click Edit enable it.
In the Edit window, input **index.html** and **error.html** into the **Index Document field** and the **Error Document field**, respectively.

  ![image](https://github.com/hhphu/Cloud/assets/45286750/20624d86-43ed-44ff-8864-4b6e6e774e6f)

Upload the content of the website onto the bucket. In my case, it's going to be the **build folder** of my React application.
Once finished, select the bucket, go to **Permissions** tab and scroll down to the bottom and visit the URL in the Bucket website endpoint.

  ![image](https://github.com/hhphu/Cloud/assets/45286750/ef5d0a4b-7749-408c-94b0-f2ac190cf913)

We should get a 403 error. This is expected because we don't have access to the file `index.html` as the bucket is set to private. To view this, we need to use CloudFront.

## Distributing content using CloudFront
Go to AWS CloudFront and create a new distribution
- Select the bucket we created above for the **Original domain** field
- Check **"Origin access control settings (recommended)** option >  click **Create new OAC**. Leave all the options as default and create a new OAC.

  ![image](https://github.com/hhphu/Cloud/assets/45286750/855c6a19-519e-46aa-bda3-c75aa5f7a5ec)

- Scroll down to the **Default cache behavior**, in the **Viewer protocol policy**, check **HTTPS only**

  ![image](https://github.com/hhphu/Cloud/assets/45286750/8629e319-bce3-49ea-97ac-56b3d515a713)

- Select **Do not enable security protections** for the WAF and **Use only North America and Europe** option for the Price class in Settings.

  ![image](https://github.com/hhphu/Cloud/assets/45286750/7e4402ad-da08-4573-acd8-8e62e17d2bdb)

- Input **index.html** into the **Default root object** field.
- Leave everything else as default and create a new distribution.
- Once a new distribution is created, we should get  yellow banner urging us to update the bucket policy

  ![image](https://github.com/hhphu/Cloud/assets/45286750/c92fc020-b23d-425e-9a33-7b70a0fd4db1)

- Go to the bucket's **Permissions** tab to update the policy

  ![image](https://github.com/hhphu/Cloud/assets/45286750/b7968cfa-9670-4f98-971a-1b3d0183110c)
