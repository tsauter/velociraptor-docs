---
title: Windows.Applications.Edge.History
hidden: true
tags: [Client Artifact]
---

Enumerate the users chrome history.


```yaml
name: Windows.Applications.Edge.History
description: |
  Enumerate the users chrome history.

parameters:
  - name: historyGlobs
    default: \AppData\Local\Microsoft\Edge\User Data\*\History
  - name: urlSQLQuery
    default: |
      SELECT U.id AS id, U.url AS url, V.visit_time as visit_time,
      U.title AS title, U.visit_count, U.typed_count,
      U.last_visit_time, U.hidden, V.from_visit, strftime('%H:%M:%f',
      V.visit_duration/1000000.0, 'unixepoch') as visit_duration,
      V.transition FROM urls AS U JOIN visits AS V ON U.id = V.url
  - name: userRegex
    default: .
    type: regex

precondition: SELECT OS From info() where OS = 'windows'

sources:
  - query: |
      SELECT * FROM Artifact.Windows.Applications.Chrome.History(
         historyGlobs=historyGlobs, urlSQLQuery=urlSQLQuery,
         userRegex=userRegex)

```
