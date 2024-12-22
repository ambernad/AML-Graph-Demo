# Anti-Money Laundering and Anti-Fraud analytics using AWS Neptune


This demo shows how to use AWS Neptune and Gremplin to effeciently find round-trippin money laundering patter in banking transactions.

<b>What is Round-Tripping?</b>

Round-tripping is a money-laundering technique where funds are cycled through multiple entities, transactions, or jurisdictions to disguise their origin and create the illusion of legitimacy. A common example is routing funds through foreign accounts or investments and then returning them as "clean" money, often exploiting tax benefits. This multi-step process can involve a web of intermediaries, making it challenging to detect with traditional databases.

<b>Implementation Guide</b>

1. Create AWS Neptune cluster using CloudFormation template from here: https://docs.aws.amazon.com/neptune/latest/userguide/get-started-cfn-create.html
2. Create new S3 bucket and upload there edges.csv and vertices.csv from this repository
3. Verify that your Neptune cluster has IAM Role that allows to read from your bucket
4. Load your data from S3 bucket to Neptune cluster using following commands:
<pre>
curl -X POST \
    -H 'Content-Type: application/json' \
    https://<YOUR-CLUSTER-DOMAIN-NAME>:<YOUR-CLUSTER-PORT>/loader -d '
    {
      "source" : "s3://<YOUR-BUCKET-NAME>/vertices.csv",
      "format" : "csv",
      "iamRoleArn" : "<YOUR-CLUSTER-IAM-ROLE-ARN>",
      "region" : "<YOUR-REGION>",
      "failOnError" : "TRUE",
      "parallelism" : "MEDIUM",
      "updateSingleCardinalityProperties" : "FALSE",
      "queueRequest" : "TRUE"
    }'
</pre>
<pre>
curl -X POST \
    -H 'Content-Type: application/json' \
    https://<YOUR-CLUSTER-DOMAIN-NAME>:<YOUR-CLUSTER-PORT>/loader -d '
    {
      "source" : "s3://<YOUR-BUCKET-NAME>/edges.csv",
      "format" : "csv",
      "iamRoleArn" : "<YOUR-CLUSTER-IAM-ROLE-ARN>",
      "region" : "<YOUR-REGION>",
      "failOnError" : "TRUE",
      "parallelism" : "MEDIUM",
      "updateSingleCardinalityProperties" : "FALSE",
      "queueRequest" : "TRUE"
    }'
</pre>

5. Launch JupytetLab in Neptune Notebooks. Import Round-tripping-seach-in-Neptune.ipynb from this repository
6. Run commands in imported notebook
