## How to use secrets manager in AWS Lambda( Node JS )

How to use secrets manager in AWS Lambda( Node JS ) 

I am going to explain how to use the AWS Secrets manager in AWS Lambda under Node JS container. This blog can be helpful for general Javascript projects also. 

### Let me first explain about AWS Secrets manager : 

**AWS secrets manager** is nothing but a locker where you can keep all secret values like important papers, jewellery ( all important secret things which you don't want to expose as publicly) and you will only have the key  to access those. In technical word AWS secrets manager manages those API keys, secrets key or client key or token or DB credentials etc. 

### Why is a secret manager important ? 

There is two use case : 

1. Sometimes it's easy to manage environment specific secret values in server side code. Because there is different server and where you can use create a environment specific values easily. But we can lose those values if we don't keep them in code and another side of the mirror is not recommended to keep those values in code or repository which can be directly exposed to developers in the production environment. 

2. Another case is client side application. It is just static code in a static file if we keep secret values it is not safe. 

Here the secret manager plays the role of lifesaver in both cases. For server side code AWS credential is able to manage and get secrets values from secrets manager. Client side you need to integrate with STS token to give temporary AWS credentials value which can only restrict for secret manager service. 

### Benefits of secrets manager : 

- Rotate secrets safely ( you can keep expiry and rotate values whenever needed )
- Manage access with fine-grained policies ( you can create a policy that enables developers to retrieve secrets values ) 
- Secure and audit secrets centrally ( it gives audit trail how many used from which account ) 
- Pay as you go ( No of secret value and no of API calls made for retrieval ) 
- Easily replicate secrets to multiple regions ( cross regions access is allow ) 

### What is require to access secrets manager : 

- AWS credentials ( combination of access key and secret key )
- AWS SDK ( server side SDK or client side SDK

I will explain how to secrets manager values in AWS Lambda for nodeJS environment. 

### How to use secrets manager in Lambda: 

AWS documentation has given a library file for a secret manager.  [JavaScript (SDK V2) Code Examples for AWS Secrets Manager ](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/secrets/secrets_getsecretvalue.js). Based on this reference I created one wrapper class `secretsManager` here is code.

1. Create a `secretssManager.js` file which will connect to `aws-sdk` to access AWS resources. 

 ```

    'use strict'

    const AWS = require('aws-sdk'); 

    class SecretsManager {
        
        /**
         * Uses AWS Secrets Manager to retrieve a secret
         */
        static async getSecret (secretName, region){
            const config = { region : region }
            var secret, decodedBinarySecret;
            let secretsManager = new AWS.SecretsManager(config);
            try {
                let secretValue = await secretsManager.getSecretValue({SecretId: secretName}).promise();
                if ('SecretString' in secretValue) {
                    return secret = secretValue.SecretString;
                } else {
                    let buff = new Buffer(secretValue.SecretBinary, 'base64');
                    return decodedBinarySecret = buff.toString('ascii');
                }
            } catch (err) {
                if (err.code === 'DecryptionFailureException')
                    // Secrets Manager can't decrypt the protected secret text using the provided KMS key.
                    // Deal with the exception here, and/or rethrow at your discretion.
                    throw err;
                else if (err.code === 'InternalServiceErrorException')
                    // An error occurred on the server side.
                    // Deal with the exception here, and/or rethrow at your discretion.
                    throw err;
                else if (err.code === 'InvalidParameterException')
                    // You provided an invalid value for a parameter.
                    // Deal with the exception here, and/or rethrow at your discretion.
                    throw err;
                else if (err.code === 'InvalidRequestException')
                    // You provided a parameter value that is not valid for the current state of the resource.
                    // Deal with the exception here, and/or rethrow at your discretion.
                    throw err;
                else if (err.code === 'ResourceNotFoundException')
                    // We can't find the resource that you asked for.
                    // Deal with the exception here, and/or rethrow at your discretion.
                    throw err;
            }
        } 
    }
    module.exports = SecretsManager;
  ```

2. Create a file for `index.js` in your Lambda package to use `secretssManager.js` class to retrieve a secret value. 

  ```
    /**
    * index.js 
    **/

    const SecretsManager = require('./SecretsManager.js');

    exports.handler = async (event) => {
        // TODO implement
        var secretName = '<secretsName>';
        var region = '<Region>';
        var apiValue = await SecretsManager.getSecret(secretName, region);
        console.log(apiValue); 
        const response = {
            statusCode: 200,
            body: JSON.stringify('Hello from Lambda!'),
        };
        return response;
    };
    ```

3. Create secret manager entry : Open this link https://console.aws.amazon.com/secretsmanager. 
![Screenshot 2021-09-25 at 12.57.09 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632511637866/-tfXi4uz8.png)
![Screenshot 2021-09-25 at 12.57.38 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632511663876/bEZAdkXd-.png)

4. Then you are done. Create a zip of this code and upload it to lambda. 

Hope this blog helps you. If you like my blog please don't forget to like the article. It will encourage me to write more such AWS Cloud related articles. You can reach out to me over my twitter handle @aviboy2006

### References: 
- https://docs.aws.amazon.com/code-samples/latest/catalog/code-catalog-javascript-example_code-secrets.html
- https://github.com/aviboy2006/aws-lambda-secrets-manager-example
