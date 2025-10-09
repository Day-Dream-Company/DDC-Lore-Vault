```mermaid
---
config:
   logLevel: 'debug'
   theme: 'base'
   gitGraph:
      rotateCommitLabel: true
---
gitGraph
   commit id: "Init"
   branch develop
   commit id: "Dev work 1"
   branch featureA
   commit id: "Feature A1"
   commit id: "Feature A2"
   checkout develop
   merge featureA
   branch release
   commit id: "Release prep"
   checkout main
   merge release
   checkout develop
   merge release
   branch hotfix
   commit id: "Hotfix 1"
   checkout main
   merge hotfix
   checkout develop
   merge hotfix
