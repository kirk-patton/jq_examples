# jq_examples

Examples

TLDR;
```
gh search code --filename=aws-rubix.yaml --json repository,path,textMatches "rubix_version" \
| jq '.[] | .repository.url as $url | .textMatches | map(select(.fragment | test("^rubix_version"))) | {url:$url,versions:(.[].fragment | split("\n"))} | {url:$url,rubix:.versions[0],eks:.versions[1]}' \
| jq -r -s '. | (map(keys) | add | unique) as $cols | map(. as $row | $cols | map($row[.])) as $rows | $cols, $rows[] | @csv'
```

CSV output

Collect Input

`gh search code --filename=aws-rubix.yaml --json repository,path,textMatches "rubix_version" > input.txt`

Input - from qh search code...

```
[
  {
    "path": "rubix/aws-rubix.yaml",
    "repository": {
      "id": "MDEwOlJlcG9zaXRvcnk0OTkwMjI1MQ==",
      "isFork": false,
      "isPrivate": true,
      "nameWithOwner": "bar/conductor-prod-machinery",
      "url": "https://..."
    },
    "textMatches": [
      {
        "fragment": "aws-rubix.yaml",
        "matches": [
          {
            "indices": [
              0,
              14
            ],
            "text": "aws-rubix.yaml"
          }
        ],
        "property": "path",
        "type": "FileContent"
      },
      {
        "fragment": "rubix_version: v0.23\neks_version: 1.29\nemail_notifications:\n",
        "matches": [
          {
            "indices": [
              0,
              13
            ],
            "text": "rubix_version"
          }
        ],
        "property": "content",
        "type": "FileContent"
      }
    ]
  },
  {
    "path": "rubix/aws-rubix.yaml",
    "repository": {
      "id": "MDEwOlJlcG9zaXRvcnk0NjM2NDc4Mg==",
      "isFork": false,
      "isPrivate": true,
      "nameWithOwner": "bar/aiml-dataplatform-ve",
      "url": "https://..."
    },
    "textMatches": [
      {
        "fragment": "aws-rubix.yaml",
        "matches": [
          {
            "indices": [
              0,
              14
            ],
            "text": "aws-rubix.yaml"
          }
        ],
        "property": "path",
        "type": "FileContent"
      },
      {
        "fragment": "rubix_version: v0.21\neks_version: 1.28\n\neks:\n  logs_retention_in_days: 90\n\ncloudwatch_logs",
        "matches": [
          {
            "indices": [
              0,
              13
            ],
            "text": "rubix_version"
          }
        ],
        "property": "content",
        "type": "FileContent"
      }
    ]
  }
]
```

First filter
`.[] | .repository.url as $url | .textMatches | map(select(.fragment | test("^rubix_version"))) | {url:$url,versions:(.[].fragment | split("\n"))} | {url:$url,rubix:.versions[0],eks:.versions[1]}`

Second Filter - slurp & raw mode
`jq -r -s '. | (map(keys) | add | unique) as $cols | map(. as $row | $cols | map($row[.])) as $rows | $cols, $rows[] | @csv'`

Output

```
"eks","rubix","url"
"eks_version: 1.25","rubix_version: v0.18","https://..."
"eks_version: 1.29","rubix_version: v0.23","https://..."
"eks_version: 1.28","rubix_version: v0.21","https://..."
"eks_version: 1.25","rubix_version: v0.18","https://..."
"eks_version: 1.28","rubix_version: v0.21","https://..."
"eks_version: 1.29","rubix_version: v0.22","https://..."
"eks_version: 1.32","rubix_version: v0.23","https://..."
"eks_version: 1.32","rubix_version: v0.23","https://..."
"eks_version: 1.29","rubix_version: v0.21","https://..."
"eks_version: 1.28","rubix_version: v0.20","https://..."
"eks_version: 1.28","rubix_version: v0.20","https://..."
"eks_version: 1.25","rubix_version: v0.19","https://..."
"eks_version: 1.28","rubix_version: v0.22","https://..."
"eks_version: 1.29","rubix_version: v0.21","https://..."
"eks_version: 1.28","rubix_version: v0.21","https://..."
"eks_version: 1.29","rubix_version: v0.22","https://..."
"eks_version: 1.29","rubix_version: v0.22","https://..."
"eks_version: 1.29","rubix_version: v0.23","https://..."
"eks_version: 1.29","rubix_version: v0.22","https://..."
"eks_version: ""1.30""","rubix_version: v0.22","https://..."
"eks_version: 1.28","rubix_version: v0.20","https://..."
"eks_version: 1.29","rubix_version: v0.23","https://..."
"eks_version: 1.29","rubix_version: v0.23","https://..."
"eks_version: 1.29","rubix_version: v0.23","https://..."
"eks_version: 1.29","rubix_version: v0.21","https://..."
"eks_version: 1.29","rubix_version: v0.22","https://..."
"eks_version: ""1.30""","rubix_version: v0.22","https://..."
"eks_version: ""1.31""","rubix_version: v0.23","https://..."
"eks_version: 1.29","rubix_version: v0.23","https://..."
"eks_version: 1.31","rubix_version: v0.23","https://..."
```
