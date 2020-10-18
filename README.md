## Setup

To setup the project on your system run:

```
bash setup.sh
```

## API Reference :

[![Run in Postman](https://run.pstmn.io/button.svg)](https://www.getpostman.com/collections/4f06dcb47916a7495bc1)

## Curl Requests

### Alert Repository

```
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"repoLink":"https://github.com/jjacob27/terraform-aws-harboroneks"}' \
  http://localhost:5000/api/v1/alert/repository

```

### Description

Gitleaks is a tool which scans a Github / Gitlab repositories for hardcoded secrets like passwords, api keys, and tokens. The curl request mentioned above uses Gitleaks to scan a repository for such exposed credentials. Sample output for a repository with exposed credentials :

```
{
    "statusCode": 200,
    "status": "success",
    "result": {
        "info": [
            [
                {
                    "line": "  AWS_ACCESS_KEY = \"AKIA6LEA7YNUAEXAMPLE\"",
                    "lineNumber": 8,
                    "offender": "AKIA6LEA7YNUAEXAMPLE",
                    "commit": "805273c8202c1e8f9c5520512fce6713ee56e66d",
                    "repo": "terraform-aws-harboroneks",
                    "rule": "AWS Manager ID",
                    "commitMessage": "New structure using modules\n",
                    "author": "Jiju Jacob",
                    "email": "jjacob@flexera.com",
                    "file": "examples/simple/main.tf",
                    "date": "2020-10-05T15:08:00+05:30",
                    "tags": "key, AWS",
                    "operation": "addition"
                }
             ]
          ],
         "leakCode": 1
        }
    }
}

```

### Alert User

```
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"userName":"jjacob27"}' \
  http://localhost:5000/api/v1/alert/user

```

### Description

Parses all the repositories of a user Github / Gitlab and searches for exposed secrets. Sample output with leaks :

```
{
    "statusCode": 200,
    "status": "success",
    "result": {
        "output": [
            {
                "info": [
                    {
                        "line": "  AWS_ACCESS_KEY = \"AKIA6LEA7YNUAEXAMPLE\"",
                        "lineNumber": 8,
                        "offender": "AKIA6LEA7YNUAEXAMPLE",
                        "commit": "805273c8202c1e8f9c5520512fce6713ee56e66d",
                        "repo": "terraform-aws-harboroneks",
                        "rule": "AWS Manager ID",
                        "commitMessage": "New structure using modules\n",
                        "author": "Jiju Jacob",
                        "email": "jjacob@flexera.com",
                        "file": "examples/simple/main.tf",
                        "date": "2020-10-05T15:08:00+05:30",
                        "tags": "key, AWS",
                        "operation": "addition"
                    }
                ],
                "url": "https://github.com/jjacob27/terraform-aws-harboroneks",
                "collaborators_url": "https://api.github.com/repos/jjacob27/terraform-aws-harboroneks/collaborators{/collaborator}",
                "teams_url": "https://api.github.com/repos/jjacob27/terraform-aws-harboroneks/teams",
                "created_at": "2020-09-25T10:54:30Z",
                "updated_at": "2020-10-05T13:37:34Z",
                "pushed_at": "2020-10-05T13:37:32Z"
            }
        ],
        "leakCode": 1
    }
}


```

Sample Output with no credentials exposed :

```
{
    "statusCode": 200,
    "status": "success",
    "result": {
        "info": [],
        "leakCode": 0
    }
}

```

## Leak Codes

| leakCode | Meaning             |
| -------- | ------------------- |
| 0        | No Leaks found      |
| 1        | Leaks found         |
| 2        | Some Error Occurred |